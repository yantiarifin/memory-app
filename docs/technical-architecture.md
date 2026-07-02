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
<br>
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

Imports, validates, and normalizes conversation data into a common internal format regardless of the source platform.

Responsibilities

* Parse exported files
* Validate data integrity
* Remove duplicates
* Normalize timestamps, authors, and message formats

Output: Standardized conversation documents



<h3>Document Processing</h3>

Breaks conversations into logical units and prepares them for analysis.

Responsibilities

* Split conversations into messages or semantic chunks
* Preserve conversation context
* Associate metadata such as date, conversation title, and source

Output: Structured conversation chunks



<h3>Chunking + Metadata Extraction</h3>

Enriches each chunk with information that can be searched and analyzed later.

Responsibilities

* Generate embeddings
* Extract topics
* Identify people, places, and organizations
* Detect goals, projects, decisions, emotions, and beliefs
* Tag each chunk with metadata

Output: Searchable knowledge objects



<h3>Knowledge Store</h3>

Stores both the original conversations and their enriched representations.

Components

* Raw conversation archive
* Vector database for semantic search
* Metadata database
* Relationship indexes

This becomes the long-term memory of DiscoveryApp.



<h3>Insight Engine</h3>

Analyzes the knowledge store to identify meaningful patterns across time.

Responsibilities

* Find recurring themes
* Detect behavioral patterns
* Identify evolving beliefs
* Surface forgotten ideas
* Connect related conversations
* Generate hypotheses supported by evidence

The Insight Engine never invents information—it synthesizes what already exists.



<h3>Evidence Layer</h3>

Every insight is backed by the exact conversation excerpts that support it.

Users can inspect:

* Which conversations contributed
* Supporting quotes
* Timeline of evidence
* Confidence level

This layer makes AI conclusions transparent and trustworthy.



<h3>DiscoveryApp UI</h3>

The interface where users explore their own history.

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