# Using MDRepo from AI agents

MDRepo runs a public [MCP](https://modelcontextprotocol.io) server at
`https://mdrepo.org/api/mcp`. AI assistants and agents can use it to search
MDRepo's simulations and read their metadata. It needs no account and is
read-only.

## Connect

- **claude.ai or Claude Desktop:** Settings → Connectors → Add custom
  connector, with the URL `https://mdrepo.org/api/mcp`. Leave the OAuth fields
  empty.
- **Claude Code:**

  ```
  claude mcp add --transport http mdrepo https://mdrepo.org/api/mcp
  ```

- **Other clients:** any MCP client that supports Streamable HTTP, with the
  same URL.

Then ask in plain language, for example "Are there MD simulations of BACE1
with a ligand bound?" or "What force field did MDR00105236 use?"

## Tools

| Tool | What it does |
|---|---|
| `search_simulations` | Search by free text, PDB ID, UniProt accession, ligand, software or collection; filter to ligand-bound or all-atom runs |
| `get_simulation` | Full metadata for one simulation: force field, water model, temperature, duration, software, replicates, RMSD/RMSF and its files |
| `find_proteins` | Proteins by UniProt accession or name, with their number of simulations |
| `find_structures` | PDB structures by ID, title or classification, with their number of simulations |
| `find_ligands` | Ligands by name or InChIKey, with their number of simulations |
| `get_stats` | MDRepo's totals |
| `prepare_download` | Check a list of simulation IDs and say how a person downloads them |

## Things to know

- **Downloading needs a person.** The server can find simulations but not
  download them. Sign in at [mdrepo.org](https://mdrepo.org) with ORCID,
  select the simulations, and download them in the browser or with
  `mdrepo get`.
- **Search ligands by InChIKey.** Names are spelled as each contributor typed
  them: "ATP" as a name finds 22 simulations, ATP's InChIKey finds 321.
  `find_ligands` gives the InChIKey.
- **One structure dominates.** Protein G B1 (PDB 1PGA) has about 4,000
  simulations, roughly 5% of the archive. Group by structure when sampling.
- **For the whole collection, use the metadata dump.** It is refreshed
  nightly at
  <https://static.mdrepo.org/metadata/mdrepo-metadata-latest.tar.gz>.
- **Scripts can use the REST API directly.** It is documented at
  <https://mdrepo.org/api/schema/swagger-ui/>.
