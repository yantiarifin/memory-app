<h1>Technical Architecture</h1>

```
User-Owned Data
        │
        ▼
Import Pipeline
        │
        ▼
Normalization Pipeline
        │
        ▼
History Store
        │
        ▼
Insight Engine
        │
        ▼
Evidence Layer
        │
        ▼
DiscoveryApp UI
```

<h2>Architecture Overview</h2>

DiscoveryApp transforms user-owned history into evidence-backed insights.

Regardless of where the history comes from, every source follows the same pipeline:

1. Import
2. Process
3. Enrich
4. Store
5. Generate insights
6. Present evidence

This architecture allows DiscoveryApp to support new data sources without changing the core insight engine.

<h2>Data Source Strategy</h2>

Initial supported source:

- ChatGPT export

Near-term supported sources:

- Claude export
- Other AI chat exports where available
- Manually uploaded text files
- Markdown notes
- PDFs or documents containing personal history

Future possible sources:

- Chrome browsing history
- Amazon purchase history
- Email exports
- Calendar exports
- Photos metadata
- Location history
- Financial or subscription history
- Other user-selected personal datasets

Important constraint:

DiscoveryApp should not depend on direct platform integrations in the early product.

The first version should support user-provided exports and uploads. This keeps the architecture simpler, avoids OAuth/platform dependency hell, and reinforces the product belief that users can actively reclaim and use their own data.

Platform connectors can come later, but they should not define the product.

<h2>Import Pipeline</h2>

Imports, validates, and normalizes user history into a common internal format regardless of the source platform.

Responsibilities

* Parse exported files
* Validate data integrity
* Remove duplicates
* Normalize timestamps, authors, and source-specific formats

Output: Standardized history records

<h2>Document Processing</h2>
Breaks imported history into logical units and prepares it for analysis.


Responsibilities

* Split conversations into messages or semantic chunks
* Preserve conversation context
* Associate metadata such as date, conversation title, and source

Output: Structured conversation chunks

<h2>Chunking + Metadata Extraction</h2>

Enriches each chunk with information that can be searched and analyzed later.

Responsibilities

* Generate embeddings
* Extract topics
* Identify people, places, and organizations
* Detect goals, projects, decisions, emotions, and beliefs
* Tag each chunk with metadata

Output: Searchable history objects



<h2>Knowledge Store</h2>

Stores both the original imported history and its enriched representations.

Components

* Raw history archive
* Vector database for semantic search
* Metadata database
* Relationship indexes

This becomes the long-term memory of DiscoveryApp.



<h2>Insight Engine</h2>

Analyzes the knowledge store to identify meaningful patterns across time.

Responsibilities

* Find recurring themes
* Detect behavioral patterns
* Identify evolving beliefs
* Surface forgotten ideas
* Connect related conversations
* Generate hypotheses supported by evidence

The Insight Engine never invents information—it synthesizes what already exists.


<h2>Evidence Layer</h2>

Every insight is backed by the exact conversation excerpts that support it.

Users can inspect:

* Which conversations contributed
* Supporting quotes
* Timeline of evidence
* Confidence level

This layer makes AI conclusions transparent and trustworthy.


<h2>DiscoveryApp UI</h2>

The interface where users explore the patterns hidden within their history.
Potential sections:

* Discover
* Timeline
* Search
* Connections
* Topics
* People
* Projects
* Beliefs
* Growth

Rather than presenting dashboards, the UI is designed to encourage exploration and self-discovery.