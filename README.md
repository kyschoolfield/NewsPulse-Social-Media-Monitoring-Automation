# NewsPulse

### Social Media News Monitoring Automation

NewsPulse is a proof-of-concept social media monitoring automation built with **Automation Anywhere A360**. The project uses REST-based RSS ingestion, XML parsing, structured data extraction, content filtering, and a modular parent/child bot architecture to collect and organize social media news content for review.

The project began as a standalone automation for monitoring content from X and later evolved into a modular architecture designed to support multiple platform-specific child bots.

> **Project Status:** Proof of Concept  
> **Implemented Platform:** X  
> **Planned Expansion:** Additional social platforms

---

## Project Overview

Monitoring social media for news content can require repetitive collection, review, and organization of posts across multiple sources.

NewsPulse was designed to explore how low-code automation could streamline that process by automatically retrieving social content, extracting relevant post information, applying basic content rules, and organizing the results into a structured audit workbook.

The initial prototype focused on X content delivered through an RSS.app feed. Rather than automating browser interactions, the bot retrieves the feed through a REST GET request and processes the resulting XML data directly.

The solution was later refactored from a standalone X automation into a parent/child bot architecture intended to support multiple platform-specific monitoring workflows.

---

## Solution Evolution

### Prototype 1: Standalone X Monitoring Bot

The original implementation operated as an independent X monitoring bot.

The automation:

1. Opened the target Excel workbook.
2. Retrieved an RSS.app feed using a REST GET request.
3. Created an XML session from the returned feed.
4. Iterated through up to 25 posts.
5. Extracted structured post information.
6. Wrote the results to the X worksheet in Excel.

Extracted data included:

- Post text
- Post link
- Publication date
- Media URL

This prototype validated the core data-ingestion and parsing workflow.

### Prototype 2: Modular NewsPulse Architecture

The solution was subsequently refactored into a parent/child architecture.

The **NewsPulse parent bot** manages the overall workflow by:

- Establishing the current system date.
- Creating a dated social-audit workbook from a template.
- Establishing the target output file.
- Invoking the X monitoring child bot.

The **X child bot** then:

- Opens the target workbook provided by the parent workflow.
- Retrieves the RSS feed through a REST GET request.
- Parses the XML response.
- Iterates through individual posts.
- Extracts structured post metadata.
- Evaluates post text for breaking-news content.
- Writes the collected information to the centralized audit workbook.

Separating orchestration from platform-specific processing made the solution more modular and created a foundation for additional monitoring components.

---

## Workflow

```text
NewsPulse Parent Bot
        |
        v
Establish System Date
        |
        v
Create Dated Social Audit Workbook
        |
        v
Pass Target File to X Child Bot
        |
        v
X Child Bot
        |
        v
RSS.app Feed
        |
        v
REST GET Request
        |
        v
XML Parsing
        |
        v
Post Data Extraction
        |
        v
Content Rule Evaluation
        |
        v
Structured Excel Output
```

---

## Content Filtering

In addition to collecting post data, the X child bot includes rule-based content filtering.

The current implementation evaluates post text for the phrase:

`Breaking News`

When the condition is met, the automation identifies the post as breaking-news content and adds it to a separate breaking-news collection.

This provided an initial method for distinguishing potentially high-priority content from routine posts.

---

## Technical Approach

### REST-Based Data Ingestion

NewsPulse uses Automation Anywhere's **REST Web Services** functionality to retrieve the RSS feed through an HTTP GET request.

This avoids dependence on browser navigation, page structure, or simulated user interaction for content collection.

### XML Parsing

The RSS response is processed through an XML session. Individual nodes are extracted during each loop iteration and assigned to variables representing the relevant post attributes.

### Structured Data Output

Extracted post information is written into a structured Excel workbook, creating a centralized record that can be reviewed or used for downstream analysis.

### Modular Bot Design

Refactoring the original standalone bot into a parent/child architecture separated workflow orchestration from platform-specific processing.

The parent bot controls the overall run and output file, while the child bot performs the X-specific ingestion and processing logic.

---

## Implemented Scope

The proof of concept successfully implemented the **X monitoring component**.

The broader NewsPulse concept envisioned additional platform-specific child bots feeding into the same centralized workflow. However, the original data-collection approach did not generalize cleanly across the other targeted social platforms because of platform and data-access constraints.

Rather than presenting those components as completed, this repository documents the implemented X workflow and the modular architecture developed to support the broader concept.

---

## Key Technical Concepts Demonstrated

- Workflow automation
- Low-code development
- REST web-service integration
- HTTP GET requests
- RSS feed ingestion
- XML parsing
- Structured data extraction
- Variable management
- Conditional logic
- Iterative processing
- Excel automation
- Parent/child bot orchestration
- Modular workflow design
- Rule-based content classification

---

## Technology Stack

| Technology | Purpose |
|---|---|
| Automation Anywhere A360 | Automation development and orchestration |
| REST Web Services | RSS feed retrieval |
| RSS.app | RSS feed generation/data source |
| XML | Feed parsing and structured data extraction |
| Microsoft Excel | Structured audit output |
| X | Implemented social content source |

---

## Challenges & Lessons Learned

### Cross-Platform Data Access

The original concept envisioned multiple social-platform child bots operating beneath the NewsPulse parent workflow.

During development, it became clear that a collection method suitable for one platform would not necessarily translate reliably to another. Differences in platform access and available data sources limited expansion of the original design.

This became an important architectural lesson: **data-access strategy should be evaluated independently from the automation logic that consumes the data.**

### From Standalone Automation to Reusable Components

The original X bot used a directly defined workbook location. During the modular redesign, the workflow was refactored so the parent bot could establish the target audit file and the child bot could operate against that target.

This reduced coupling between the X-specific processing logic and the overall NewsPulse workflow.

### Rule-Based Classification Has Limits

Keyword detection provided a simple mechanism for identifying posts containing **"Breaking News,"** but it relies on exact textual patterns.

A post can describe a significant developing event without explicitly using those words, while another post may contain the phrase without representing genuinely high-priority news.

This limitation provides a natural opportunity for semantic classification in a future version of the project.

---

## Future Enhancement: NewsPulse v2

A future version of NewsPulse could modernize the original architecture using **Python, supported APIs or data feeds, JSON-based processing, and an LLM for semantic content analysis**.

A potential architecture is:

```text
Supported API / Data Source
          |
          v
        Python
          |
          v
     JSON Processing
          |
          v
   LLM Classification
          |
          v
Summary & Prioritization
          |
          v
Structured Storage / Logging
```

Instead of relying primarily on keyword matching, an LLM-enabled version could classify content semantically into categories such as:

- Breaking news
- Developing story
- Routine update
- Promotional content
- Other

Additional enhancements could include:

- Automated summarization
- Priority scoring
- Topic classification
- Duplicate detection
- Structured JSON output

NewsPulse v2 would preserve the original project's core objective while exploring a more flexible approach to data ingestion and intelligent content analysis.

---

## Repository Notes

Screenshots and examples included in this repository are sanitized to remove private file paths, feed identifiers, internal information, and environment-specific details.

No credentials, private tokens, proprietary data, or internal system information are included.

---

## Project Context

NewsPulse was developed as a hands-on automation project exploring REST-based data ingestion, structured content processing, modular bot architecture, and automated news monitoring.

The project also serves as a foundation for continued development in **API integration, Python automation, and LLM-enabled workflow design**.
