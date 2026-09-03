# NewsPulse

### Social Media News Monitoring Automation

NewsPulse is a proof-of-concept social media monitoring automation built with Automation Anywhere A360. The project uses REST-based RSS ingestion, XML parsing, structured data extraction, content filtering, and a modular parent/child bot architecture to collect and organize social media news content for review.

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

---

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
