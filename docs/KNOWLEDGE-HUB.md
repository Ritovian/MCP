# Ritovian Knowledge Hub

Status: agreed product direction; implementation planned.

The hub lets organisations joining Ritovian contribute religious knowledge for public readers and MCP clients. Authors can use that knowledge as evidence in their blog posts and optionally share the full post on the Ritovian hub.

The [release plan](RELEASE-PLAN.md) schedules the public knowledge MCP launch after the Ritovian application release and a working hub pilot.

## 1. Confirmed decisions

- Organisations contribute through their existing Ritovian account and team.
- Published knowledge feeds both public hub pages and MCP retrieval.
- Knowledge is attributed to its contributor and carries religious context.
- Authors back particular points in their blog posts with citations.
- A citation identifies a passage and a specific published knowledge revision.
- Organisations choose hub article sharing per post; it starts off.
- A shared post appears as a full article with its author, organisation, references and original website link.
- The existing blog editor remains the editing workflow for both article destinations.

## 2. Knowledge and religious categories

Each entry has a title, summary, body, contributor, author, source language, religious scope, topics and sources. Sections or passages have stable identifiers within a revision so a citation can resolve to the relevant text.

Reuse the application's tradition, family and denomination registry. Add shared knowledge topics, such as beliefs and teachings, practice and worship, festivals and calendars, history, terminology and visiting.

The organisation's tradition is the initial selection. Entries can identify more specific denominations, local practice, or several relevant traditions for a comparative explanation. Readers should be able to distinguish a community's account of its own practice from an explanation with a wider scope.

Knowledge coverage is tracked independently from platform onboarding availability. A tradition can have published knowledge while some organisation-management features for that tradition are still in progress.

## 3. Organisation workflow

Add **Knowledge** to the organisation dashboard:

1. Create an entry and choose its context and topics.
2. Write the explanation and attach sources to relevant passages.
3. Preview the attribution, references and public presentation.
4. Publish a revision to the hub.
5. Edit a new draft when the explanation needs updating.
6. Publish the next revision, or withdraw the public entry when necessary.

Use the existing organisation team and introduce explicit knowledge-editing, knowledge-publishing and hub-sharing capabilities. Define their defaults against the existing owner, admin and editor roles. Drafts and unpublished edits stay within the organisation's workspace.

Publication creates a public revision. Both the website and MCP use its published representation. Publishing an update refreshes the current revision, search results and related public caches together.

## 4. Sources and blog citations

### Original references

A source record can contain:

- Title and author or issuing organisation.
- Source type, such as scripture, official publication, research or community practice.
- URL or bibliographic reference.
- Publication date where known, and date accessed where relevant.
- Page, chapter, verse, section or timestamp.
- An optional short quotation where the contributor has permission to include it.

### In the blog editor

Add a **Sources & evidence** panel. An author selects a sentence or paragraph, searches the public hub, chooses the supporting passage, and inserts a reference. Authors can also add an original external reference directly.

Start the search with the organisation's religious context selected, while allowing the author to broaden it. Another organisation's published entries can be cited with their original attribution.

Published posts show inline reference markers and a Sources section. A reference preserves the public entry identifier, revision, passage identifier and relevant source details. The citation chain is:

```text
Blog statement
    -> knowledge passage in a published revision
        -> original reference and precise location
```

An update to a knowledge entry does not silently change existing citations. Authors can compare the new revision and choose to update their references. If an entry is withdrawn, its public reference resolves to a withdrawal notice, its content leaves active retrieval, and affected authors are notified. Withdrawal state takes precedence over a request for an older revision.

## 5. Full articles on the hub

Extend the existing blog publishing controls with **Also publish on the Ritovian hub**. This is a separate, per-post choice from publishing on the organisation's website.

A hub publication includes the full article, author, contributing organisation, religious context, topics, citations, publication dates and original URL. The original organisation URL is the canonical URL for the mirrored article.

Publishing a new version of the source post updates the hub publication. Saving a draft does not change the public article. Scheduled posts appear on either destination only when their publication time arrives.

Turning off hub sharing removes the hub publication while retaining the organisation's website article. Unpublishing the original withdraws its mirrored hub article. A withdrawn citation is labelled within an otherwise published article and brought to the author's attention.

Hub media must resolve on the platform host. The publication process needs an explicit public media mapping or snapshot, rather than relying on an organisation-scoped `/media/` path resolving on another host.

## 6. Public experience

The proposed public area is `/hub`, with:

- Knowledge search and entry pages.
- Full articles shared by organisations.
- Tradition, denomination, topic, language and contributor filters.
- Contributor profiles linking to an organisation and its public contributions.
- Visible sources, revision information and related knowledge.

Knowledge and articles retain distinct content types in public search and MCP results. An article can explain an organisation's perspective while linking to the knowledge it cites.

## 7. Proposed implementation

The implementation belongs in the sibling `Application` repository. Its existing Prisma data layer, organisation tenancy, blog editor, tradition registry and hosted MCP endpoints provide the foundations.

The following names describe the proposed responsibilities; the application migration will finalise the schema.

| Addition | Responsibility |
| --- | --- |
| `KnowledgeEntry` | Organisation-owned working entry, authorship, religious context and publication pointers. |
| `KnowledgeRevision` | Immutable authored text and passage identifiers for a revision and language. |
| `KnowledgeSource` | Reusable original references available to the organisation's authors. |
| `KnowledgeCitation` | Links between knowledge passages and original references. |
| `PublishedKnowledgeRevision` | Deliberately public snapshot of an entry revision, its attribution and source references, with publication and withdrawal state. |
| `PostCitation` | Links between blog passages and public knowledge revisions or direct original references. |
| `HubPublication` | Public full-article snapshot, originating organisation/post, citations, media mapping and update state. |
| Shared knowledge topics and associations | Browsing and filtering across contributing organisations. |

Organisation-owned working content follows the application's tenant-scoped Prisma and row-level-security conventions. Public readers use a dedicated published-content service over explicitly public snapshots. That service is shared by hub pages, search and MCP.

A publication transaction records the published revision and its source snapshot. Public reads check current publication state, including when a caller requests a historical revision or receives an older search result. Publishing, withdrawing and unsharing also refresh relevant search indexes, caches and sitemaps.

Translations identify the same public entry while retaining their own revision and language. Retrieval reports the actual returned language and any fallback. A translated passage must still resolve to the revision and references used by its citation.

## 8. Planned MCP contract

Extend the existing platform MCP endpoint with these proposed tools:

| Tool | Purpose |
| --- | --- |
| `search_knowledge` | Find published entries by query, religious context, topic, language and contributor, with pagination. |
| `get_knowledge_entry` | Retrieve an entry's published text, requested revision where available, passage identifiers and original sources. |
| `search_hub_articles` | Find full articles that organisations have explicitly shared to the hub. |
| `get_hub_article` | Retrieve a published article, its original URL and its citation chain. |

These names are proposed until the implementation and beta establish the released contract. Existing platform-information and organisation tools continue to serve their current purposes.

Results include the content type, public identifier, revision, contributor, religious scope, actual language, public URL, publication/update dates and source references. Search results carry concise excerpts; detail tools return bounded text with pagination or explicit continuation when needed.

MCP retrieves published material when an assistant requests it. Religious context, contributor attribution and original reference URLs should remain available throughout the response.

## 9. Delivery sequence

1. **Contribute and discover:** entry editor, sources, revisions, public knowledge pages and search.
2. **Cite:** blog citation picker, inline references, Sources section and revision-update handling.
3. **Share:** optional full-article publication, canonical links, media mapping, updates and withdrawals.
4. **Retrieve:** hub MCP tools using the same published-content service.
5. **Release:** pilot, client beta, registry metadata and public documentation as described in the [release plan](RELEASE-PLAN.md).

The first complete journey is an organisation contributing an explanation, citing it in a blog post, sharing the full article to the hub, and retrieving the published knowledge through MCP.

## 10. Decisions to settle during implementation

- Contributor content-reuse terms, including permission to display material publicly and return it through MCP. Repository code licensing and contributor content terms have separate scope.
- Behaviour when a contributing organisation closes: retained attribution, withdrawal and references from other organisations.
- Initial supported languages and the presentation of translated or fallback content.
- The founding contributor cohort and the actual topics/coverage to announce at launch.

## 11. First-release acceptance examples

- An organisation publishes a sourced knowledge entry that appears identically in public detail retrieval and MCP, with the same revision and attribution.
- A second organisation can cite that public revision while each organisation retains its own draft workspace.
- Updating an entry leaves the earlier citation traceable; withdrawing it removes the text from active retrieval and labels the reference.
- A post can remain on the organisation's website alone, or also appear in full on the hub when its author enables sharing.
- Draft edits, scheduled publication, published updates and withdrawal behave consistently across the public hub and MCP.
