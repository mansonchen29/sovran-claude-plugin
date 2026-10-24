---
name: video-ads
description: Create, review, and export video ad drafts with the connected Sovran tools. Use when the user asks to make ad variations, remixes, audience ads, or Creative Copilot edits from media saved in Sovran, or to check Sovran jobs, drafts, estimates, renders, or credits.
---

# Sovran video ads

Sovran turns media saved in a customer's projects into editable video ad drafts. Work only through the `sovran_*` tools of the connected Sovran server. Each named tool result has a `guidance` object. Follow its `nextCalls`, `reviewRequired`, `stopPolling`, and `notes`.

## Find the workspace and sources

1. Call `sovran_get_profile` to confirm the connected workspace.
2. Call `sovran_list_projects`. If several projects match the user's words, ask which one to use.
3. Call `sovran_list_assets` for that project. For speech flows, set `query.requireTranscript` to `true`.

An empty list means the media is missing. Say so and stop. Never invent project, asset, draft, or job IDs. The tools cannot read local files. The user uploads media in the Sovran dashboard or with the Sovran CLI (https://sovran.ai/docs/api/cli).

## Choose the flow

Use the flow the user asked for. Ask when the flow or the source is materially unclear. Do not substitute one flow for another.

| The user wants | Tool | Select first | Count when unstated |
|---|---|---|---|
| Variations of one talking video | `sovran_create_video_variations` | One video with ready speech | 4 |
| Many ads from all speech footage in a project | `sovran_create_smart_variations` | Check sources with `sovran_preview_smart_sources` | 5 |
| Ads for specific audiences | `sovran_find_audiences`, then `sovran_create_audience_variations` | One video of 10 minutes or less, and 1 to 3 audiences the user approves | 1 per approved audience |
| Chosen hook, body, and CTA clips combined (Modular Builder, Arrange clips) | `sovran_create_manual_variations` | Ordered blocks of asset IDs | Set by the blocks |
| New ads from a saved voiceover | `sovran_create_voiceover_remix` | 1 to 3 succeeded voiceovers from `sovran_list_voiceovers` | 1 per voiceover |
| B-roll ads from footage and a brief | `sovran_create_broll_remix` | Footage and an ad brief | 1 per hook |
| An edit made with a template or instructions | `sovran_create_creative_copilot` | A template from `sovran_list_copilot_templates`, or custom instructions | Template default |

Keep saved template settings and server defaults for other omitted fields.

## Stop for the user three times

1. **Audiences.** After `sovran_get_audience_analysis` reports `ready`, show each audience with its name, callout, and evidence. Create audience drafts only for the 1 to 3 audiences the user approves.
2. **Drafts.** Creation ends at editable drafts, or at saved combinations for a sequence. Show each draft name and its editor link. Ask which drafts to export. An editor link is not a finished video.
3. **Cost.** Estimate only the selected drafts or combinations. Show `outputCount` and `creditCost`. Export only after the user approves that cost, or when it is within a budget the user already set.

Never export automatically after creation.

## Export the selected outputs

| Selection | Estimate | Export | Progress |
|---|---|---|---|
| One draft | `sovran_estimate_draft_export` | `sovran_export_draft` | `sovran_get_job` |
| 2 to 10 drafts from one project | `sovran_estimate_drafts_export` | `sovran_export_drafts` | `sovran_get_drafts_export` |
| Sequence combinations | `sovran_estimate_sequence_export` | `sovran_export_sequence` | `sovran_get_job` |

Submit the export with the estimate's `planFingerprint` and the same selection and settings. `guidance.exportPlan` holds those arguments. A changed selection or a revision conflict needs a new estimate. Read finished videos with `sovran_get_render_outputs`. An expired link needs another read of the output, not another render.

## Make changes once

Every change takes a top-level `idempotencyKey`. Create a new UUID for each new request. Keep it with the exact tool arguments. Reuse that key only with the same arguments, to read the saved result of a request whose outcome is unknown. Do not retry automatically. Do not start another creation while an earlier result is unknown.

## Check jobs

Creation and export return immediately with a public job ID. Read `sovran_get_job` only while the status is `queued`, `running`, or `waiting`. Read about ten seconds apart and at most six times in one turn. Then give the user the job ID to check later. A chat that ends does not cancel the job.

Stop reading at any other status. Follow `nextAction` and `retryAllowed`. Read child jobs before you report a count. `partially_succeeded` means some outputs failed, so report the actual successes and failures. A batch ID or a native Smart run ID is not a job ID.

## Permissions

The connection starts with Read. Write, Generate, Delete, and Publish are requested when a task needs them. If a tool reports missing permissions and no sign-in prompt opens, ask the user to reconnect Sovran and approve the named permissions. No action ran. Creation and export use the workspace's plan, credits, and role.
