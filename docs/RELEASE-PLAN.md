# MCP release plan

Status: planned. The public knowledge MCP release follows the Ritovian application launch and a working knowledge-hub pilot.

**Release sequence: application launch → hub pilot → MCP beta → public MCP release → ongoing updates.**

This is a readiness-based sequence. There is no committed calendar date or released knowledge-tool version yet. The [knowledge hub plan](KNOWLEDGE-HUB.md) records the product scope.

## 1. What exists today

The application already contains the hosted MCP infrastructure:

| Area | Application location / current value |
| --- | --- |
| Hosted route | `src/app/mcp/route.ts`, also exported at `/api/mcp`. |
| Platform tools | `src/server/mcp/platform.ts`: platform information, traditions, features, plans and help. |
| Organisation tools | `src/server/mcp.ts`: an organisation's published content and enabled modules. |
| Identity and version | `src/server/mcp/identity.ts`; platform name `com.ritovian/ritovian`, current source version `2.0.0`. |
| Discovery metadata | `src/server/mcp/discovery.ts`: server card, registry document and AI catalogue. |
| Response format | Text and structured content, with public read-only tool annotations. |
| Transport | Stateless Streamable HTTP. |

These are source-code observations, not a claim that the production endpoint or an official registry listing has been verified. The contributor knowledge hub, blog citations, full-article sharing and hub retrieval tools remain planned additions.

The intended platform endpoint is `https://ritovian.com/api/mcp`. It becomes the published connection URL after production verification. Organisation-specific endpoints retain their own scope and identity.

## 2. Release responsibilities

| Work | Responsible role |
| --- | --- |
| Application deployment and hub implementation | Application maintainers. |
| Initial knowledge, sources and article contributions | Founding contributing organisations. |
| MCP tool contract, client checks and discovery metadata | MCP/application maintainers. |
| Public documentation, GitHub release and registry publication | MCP repository maintainers. |

Record the application commit, public documentation commit, runtime MCP version and demonstrated coverage in the release notes so each release is reproducible.

## 3. Stage A — Ritovian application launch

The application launch is the starting point for the public knowledge MCP rollout. The application's deployment sequence is maintained in `Application/docs/launch.md` in the sibling checkout.

- [ ] Ritovian is deployed on its intended production domain.
- [ ] The first organisation is live and its team can use the publishing workflow.
- [ ] Existing platform and organisation MCP endpoints have been exercised against production, including their host-specific identities and discovery documents.
- [ ] The production application commit and existing MCP tool list are recorded.

Outcome: a released platform on which organisations can contribute. The knowledge service is described as planned until its hub features are ready.

## 4. Stage B — Knowledge hub pilot

Run the first complete contribution journey after application launch. Start with a small founding cohort, including at least two organisations so cross-organisation citations and contributor attribution can be exercised.

- [ ] Organisation teams can create, source, preview, publish and update knowledge entries.
- [ ] Public hub pages expose the published revision with its contributor, religious scope and original sources.
- [ ] Authors can cite a particular knowledge revision and passage in their blog posts.
- [ ] An organisation can optionally share a full article, with citations and a canonical link to its original website.
- [ ] Updates, scheduled publication, unsharing and withdrawals produce the intended public state.
- [ ] At least one end-to-end example is ready to use in documentation and client testing.
- [ ] Initial topic/language coverage and the contributor content-reuse terms are published with the hub.

Outcome: the public knowledge and article service works for real contributors before it is promoted as an MCP knowledge source.

## 5. Stage C — MCP beta after the hub pilot

Implement the proposed knowledge and article retrieval tools against the same published-content service used by the hub. Keep the runtime tool list authoritative; document only tool names and inputs that have been deployed.

The beta is a limited announcement to testers using the hosted URL. Public read-only tools remain publicly callable; an unlisted URL is not an access restriction.

### Retrieval checks

- [ ] Search and detail tools return the expected published knowledge and shared full articles.
- [ ] Results retain the contributor, religious scope, revision, language, public URL and original references.
- [ ] A requested historical revision respects the entry's current withdrawal state.
- [ ] Drafts, unpublished edits, future-dated articles and unshared posts are absent from public retrieval.
- [ ] Pagination, empty results, unsupported filters, unknown identifiers and language fallback have documented behaviour.
- [ ] Updating or withdrawing content refreshes hub pages and MCP retrieval consistently, including stale search results.

### Client and hosting checks

- [ ] Exercise connection, initialization, tool discovery and representative tool calls with an MCP inspector or SDK client.
- [ ] Check a current supported Claude client and at least one editor client, such as Cursor or VS Code; record the client versions and date.
- [ ] Confirm source links and inline citation destinations resolve from a returned result.
- [ ] Confirm browser-client CORS behaviour where supported, plus rate-limit responses and retry information.
- [ ] Record supported protocol versions from the deployed server, representative response sizes, errors and latency.

Outcome: a demonstrated client/tool compatibility matrix and a stable initial public contract.

## 6. Stage D — Prepare the public release

### Version and metadata

The application's `MCP_VERSION` is the runtime source of truth. It is `2.0.0` in the source reviewed for this plan. Choose the next version against the actual changes: additive compatible tools normally warrant a minor increment; breaking tool contracts warrant a major increment.

Use that same version in the runtime handshake, discovery metadata, repository `server.json`, client manifests that carry a version, changelog and GitHub release tag. A new documentation repository does not restart the existing server's version history.

- [ ] Finalise tool names, inputs, outputs and advertised coverage from beta results.
- [ ] Update the runtime description to reflect the knowledge capabilities actually shipping.
- [ ] Validate the live registry document and server card against their current published schemas.
- [ ] Add a matching root `server.json` to this repository with the hosted remote, repository URL and release version.
- [ ] Verify control of the domain-backed `com.ritovian/ritovian` registry namespace using the registry's current domain-authentication process.
- [ ] Prepare client configuration examples and any client manifests exercised successfully in beta.

The release is a hosted Streamable HTTP remote. Registry metadata describes that deployed service; a standalone package is a separate distribution decision.

### Documentation

- [ ] Replace the README's planned connection section with the verified URL and tested instructions.
- [ ] Publish the released tool list, supported filters, pagination and source-reference format.
- [ ] List actual knowledge coverage separately from the platform's supported organisation traditions.
- [ ] Describe public access, current rate limits and where users can report incorrect content or connection problems.
- [ ] Move the shipped changes from Unreleased to a dated changelog entry.
- [ ] Include the application commit, release version and any remaining limitations in the release notes.

Outcome: a release candidate whose runtime, metadata and documentation describe the same service.

## 7. Stage E — Publish the MCP release

Carry out the following in order:

1. **Deploy the release candidate** in the application and verify the production handshake, tool list, discovery documents and representative source-backed answers.
2. **Freeze the repository release files** against that deployed version, including `server.json`, the connection guide and changelog. Commit the release documentation.
3. **Publish the remote's metadata to the official MCP Registry** using the current `mcp-publisher` release and the authenticated Ritovian domain namespace.
4. **Read the published registry entry back** and compare its name, version, remote URL and description to the deployed service and repository manifest.
5. **Tag the documentation commit** with `v` followed by the exact MCP version and create the GitHub release, linking the corresponding application commit.
6. **Announce availability** on Ritovian and to contributing organisations, with tested connection instructions and a small set of supported example questions.

Only advertise tools and tradition coverage demonstrated by the released service. Additional client-directory submissions can follow as those clients are verified.

Outcome: a discoverable hosted MCP service with matching runtime, registry and repository release information.

## 8. After release

- Watch tool-call success, errors, latency, rate limiting and empty-result patterns during the first normal contributor/client usage cycle.
- Re-run representative retrieval checks after publication changes, particularly source revisions and withdrawals.
- Keep a visible correction path from a knowledge entry to its contributing organisation.
- Use the changelog for tool-contract changes and notable coverage additions; individual article edits retain their content revision history.
- For subsequent MCP releases, deploy and verify the new runtime, then update registry metadata, manifests and release notes together.

### Recovery

If a tool deployment fails, restore the previous compatible application build and verify its tool contract. Keep schema changes backwards-compatible with that recovery build where possible.

If a knowledge item is withdrawn, remove its text from active retrieval, preserve the reference's withdrawal status and notify affected authors. If a hub article is unshared, remove the mirrored article from public search and MCP.

If published metadata is wrong, correct it with a fresh version in accordance with the registry's current update rules. Update the connection documentation if availability changes.

## 9. References

- [Knowledge hub product and implementation plan](KNOWLEDGE-HUB.md).
- [MCP Registry: remote servers](https://modelcontextprotocol.io/registry/remote-servers).
- [MCP Registry: publisher quickstart](https://modelcontextprotocol.io/registry/quickstart).
- [MCP Registry: authentication](https://modelcontextprotocol.io/registry/authentication).
- [MCP Registry: version management](https://modelcontextprotocol.io/registry/versioning).

Registry and client details should be checked against their current documentation when executing the release.
