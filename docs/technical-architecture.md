```text
AI Conversation History
         │
         ▼
Import Pipeline
         │
         ▼
Document Processing
         │
         ▼
Chunking + Metadata Extraction
         │
         ▼
Knowledge Store
(Vector DB + Metadata)
         │
         ▼
Insight Engine
         │
         ▼
Evidence Layer
         │
         ▼
MeTapestry UI
```


<h3>AI Conversation History</h3>

The raw source material for MeTapestry. Users import conversation exports from AI platforms such as ChatGPT and Claude. These conversations become the foundation for all future analysis.

Input: Conversation export files



<h3>Import Pipeline</h3>

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

This becomes the long-term memory of MeTapestry.



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



<h3>MeTapestry UI</h3>

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