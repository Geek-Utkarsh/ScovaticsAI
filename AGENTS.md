<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.

## What Lovable manages

Rules outside this block are the project's own. Change the pieces below in
Lovable, not by hand. With the [Lovable MCP](https://mcp.lovable.dev)
connected, `send_message` asks Lovable to do it; otherwise ask the user to
use the editor.

- **Database.** Do not add or edit `supabase/migrations/` or `drizzle/`.
  Ask Lovable to change the schema.
- **Generated files.** Do not edit a file whose header says it is automatically
  generated.
- **AI gateway.** Keep model calls on Lovable's gateway, and keep
  `LOVABLE_API_KEY` out of browser code.
- **Connectors.** Lovable provides native connectors. `list_connectors` shows
  which are enabled; `add_connector` only returns the dashboard link. When a
  connector is enabled, calls go through the connector gateway and need
  `LOVABLE_API_KEY`. When Lovable does not provide one, users can bring their
  own key and store it in Secrets.
- **Secrets.** Lovable manages them. Do not put secret values in the repo.
<!-- LOVABLE:END -->

## Scovatics architecture
- Visitor assessment/report/enquiry writes go through public server functions using the admin client and an unguessable `access_token`; tables have no anon policies, so visitors can never read others' data.
- AI calls live only in `src/lib/ai.server.ts` (Responses API, structured zod output); prompts and the curated `use_case_library` ground recommendations.
- Admin server functions check `has_role(admin)` via the caller's own client before any read/write; the first signed-in user may claim admin only while no admin exists.
- Service and industry copy is static in `src/content/`; blog, FAQ, case studies and use cases are database-managed from /admin.
- No gradients anywhere in the design system; colors come only from tokens in `src/styles.css`.
