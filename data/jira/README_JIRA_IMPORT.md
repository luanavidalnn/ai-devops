# Jira bulk import CSV files

| File | Use case |
|------|----------|
| `jira_poc_bulk_import.csv` | Broader AI-assisted DevOps POC (incident + CI/CD) |
| `jira_process_improvement_tracks.csv` | Process improvement stories linked to that epic |
| `jira_release_change_intelligence_poc.csv` | **Epic + issues** for the Release Change Intelligence POC (Point 1) |

## Import notes (Jira Cloud / Data Center)

1. **Administration** → **System** (Cloud: **Jira settings** → **Apps** or use **CSV import** under Issues where available).  
2. Map columns: Summary, Issue Type, Description, Labels, Story Points, **Epic Name** / **Epic Link** per your project’s import wizard.  
3. For Epic association, the child rows use **Epic Link** = the same string as **Epic Name** on the Epic row (`POC: Release Change Intelligence (DevOps AI Point 1)`). Adjust the epic title in both columns if your import expects an Epic **key** instead (then re-link issues after import).  
4. After import, move issues to your **Kanban board** backlog and link the Epic on the board if your project uses Epic panel filters.

If your importer does not support `Epic Name` on the Epic row, create the Epic manually in Kanban, then bulk-create tasks and set **Epic Link** to that Epic in the UI or with a second CSV import using the Epic issue key.
