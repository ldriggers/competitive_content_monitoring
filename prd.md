# PRD: Competitor sitemap monitor

## 1. Product overview
### 1.1 Document title and version
- PRD: Competitor sitemap monitor
- Version: 1.0

### 1.2 Product summary
The Competitor Sitemap Monitor is a lightweight business intelligence system designed to track changes to competitors' websites through their XML sitemaps. The system will automatically fetch sitemap data from defined competitor domains, identify new, updated, and removed URLs, and maintain a historical record of these changes.

This initial version will focus on monitoring XML sitemaps from two key competitors (Bankrate.com and NerdWallet.com), tracking URL metadata, and detecting changes without storing the actual content of the pages. The system will run daily via GitHub Actions to ensure continuous monitoring with no external infrastructure costs.

## 2. Goals
### 2.1 Business goals
- Gain competitive intelligence on when and where competitors update their content
- Identify content publication patterns and frequencies
- Detect new content areas or categories being developed by competitors
- Create an early warning system for competitor content strategy changes
- Reduce manual competitive research time by automating change detection

### 2.2 User goals
- Receive reliable data about competitor website changes without manual checking
- Understand which specific URLs have been added, modified, or removed
- View historical patterns of content updates over time
- Correlate business initiatives with competitor content activities
- Save time by automating repetitive competitive monitoring tasks

### 2.3 Non-goals
- Analyzing actual content changes within pages (future phase)
- Providing recommendations on content strategy
- Monitoring more than two competitors initially
- Tracking data sources beyond XML sitemaps (RSS, category pages, etc.)
- Building a user interface or dashboard in this phase
- Integrating with external data sources or business intelligence tools

## 3. User personas
### 3.1 Key user types
- Content strategists
- SEO specialists
- Product managers
- Marketing analysts
- Competitive intelligence professionals

### 3.2 Basic persona details
- **Content Strategist**: Marketing professional responsible for planning and developing content strategy for the organization, who needs to know what topics and content types competitors are focusing on.
- **SEO Specialist**: Digital marketing professional who optimizes website content for search engines and tracks competitor SEO activities and content updates.
- **Product Manager**: Team lead responsible for product offerings who monitors competitor moves to inform product roadmap decisions.
- **Marketing Analyst**: Data-focused marketing team member who analyzes market trends and competitor activities to inform marketing strategy.
- **Competitive Intelligence Professional**: Specialist focused exclusively on gathering and analyzing competitor information for strategic decision-making.

### 3.3 Role-based access
- **Administrator**: Can configure which domains to monitor, access all data, and manage system settings.
- **Analyst User**: Can view and analyze all collected data but cannot modify system configuration.
- **Stakeholder**: Can view simple reports and summaries of competitor change activity.

## 4. Functional requirements
- **Sitemap fetch and parse** (Priority: High)
  - System must fetch XML sitemap from configured domains
  - System must handle sitemap index files that point to multiple sitemaps
  - System must extract URL, lastmod, priority, and changefreq values from sitemaps
  - System must handle different sitemap formats and standards

- **Change detection** (Priority: High)
  - System must identify new URLs that did not exist in previous runs
  - System must identify modified URLs where lastmod date has changed
  - System must identify removed URLs that were present in previous runs but not in current run
  - System must store historical record of all changes with timestamps

- **Data storage** (Priority: High)
  - System must store URL metadata in Parquet format for efficiency
  - System must track when URLs were first and last seen
  - System must maintain a change log of URL modifications

- **Scheduling** (Priority: Medium)
  - System must run automatically once per day via GitHub Actions
  - System must be manually triggerable for on-demand runs

- **Configuration** (Priority: Medium)
  - System must allow addition or removal of domains to monitor through config file
  - System must support custom user-agent strings for ethical crawling

- **Error handling** (Priority: Medium)
  - System must handle and log network errors when fetching sitemaps
  - System must continue operation even if one domain fails
  - System must implement retries for transient errors

## 5. User experience
### 5.1. Entry points & first-time user flow
- **Repository setup**: Users clone the GitHub repository to their local machine
- **Configuration**: Users modify config.json to specify domains to monitor
- **Initial run**: Users trigger first run manually to establish baseline data
- **Automated monitoring**: System continues to run daily after initial setup
- **Data access**: Users access Parquet files directly or through Python scripts for analysis

### 5.2. Core experience
- **Setup configuration**: The user defines which competitors to track by editing the config.json file with domain names and sitemap URLs.
  - The configuration should be simple JSON format with clear documentation.
- **View changes**: Users examine the url_changes.parquet file to see what URLs have been added, updated, or removed.
  - This data should be easily readable using pandas or similar tools with minimal code.
- **Analyze patterns**: Users analyze historical data to identify patterns in content publishing.
  - The data structure should support simple time-series analysis.
- **Integrate findings**: Users incorporate insights into their content and marketing strategies.
  - The raw data should be exportable to various formats for inclusion in reports.

### 5.3. Advanced features & edge cases
- **Handling large sitemaps**: System must efficiently process very large sitemaps or those split across multiple files
- **Handling irregular update patterns**: System should track if a competitor suddenly changes their update frequency
- **Detecting sitemap format changes**: System should adapt if a competitor changes their sitemap structure
- **Handling temporary outages**: If a competitor's sitemap is temporarily unavailable, system should retry and not lose historical data

### 5.4. UI/UX highlights
- **Structured data output**: Data stored in an organized format for easy processing
- **Simple configuration**: JSON-based configuration that is human-readable and editable
- **Actionable logs**: Clear logging of activities and changes for troubleshooting
- **Command-line accessibility**: All functions accessible via simple commands
- **Minimal dependencies**: Easy setup with few external requirements

## 6. Narrative
Sarah is an SEO specialist at a financial services company who wants to stay ahead of competitor content strategies because her team has limited resources and needs to prioritize their content creation efforts. She finds the Competitor Sitemap Monitor and sets it up to track Bankrate and NerdWallet automatically. Each morning, she receives an automated update of all the new and changed URLs from these competitors, allowing her to quickly identify new content areas they're focusing on and prioritize her team's response without manually checking dozens of pages daily.

## 7. Success metrics
### 7.1. User-centric metrics
- Reduction in time spent manually checking competitor websites (target: 80% reduction)
- Increase in competitor updates discovered (target: detect 95% of all sitemap changes)
- User satisfaction with system reliability (target: 99% scheduled runs complete successfully)
- Speed of identification of new competitor content (target: under 24 hours from publication)

### 7.2. Business metrics
- Number of competitive insights generated from the data (target: 5+ actionable insights per month)
- Impact on content strategy decisions (target: influence 30% of content planning)
- ROI based on time saved vs. manual monitoring (target: 10x return on development time)
- Improvement in time-to-market for competitive responses (target: 50% reduction)

### 7.3. Technical metrics
- System reliability (target: 99.5% uptime)
- Data accuracy (target: 99.9% of sitemap changes correctly identified)
- Processing efficiency (target: complete full monitoring cycle in under 5 minutes)
- Storage efficiency (target: maintain repository size under 200MB for 6 months of historical data)

## 8. Technical considerations
### 8.1. Integration points
- GitHub Actions for scheduling and execution
- Git for version control and data storage
- Python ecosystem for data processing
- Parquet format for data storage and retrieval
- XML processing for sitemap parsing

### 8.2. Data storage & privacy
- All data stored in Parquet files within GitHub repository
- No storage of actual page content, only metadata
- Data limited to publicly available sitemap information
- No personally identifiable information collected
- Repository can be private to restrict access to data

### 8.3. Scalability & performance
- Parquet format enables efficient storage and queries
- Process modularized to handle additional domains with minimal changes
- Rate limiting and politeness built in for web requests
- Incremental processing approach to handle growing data volumes
- Design allows future migration to external database if needed

### 8.4. Potential challenges
- GitHub storage limitations if monitoring many domains long-term
- Competitors changing sitemap formats or URLs without notice
- Rate limiting or blocking of automated requests by some websites
- Handling of very large sitemaps with thousands of URLs
- Managing complex sitemap structures (multiple levels of sitemapindex)

## 9. Milestones & sequencing
### 9.1. Project estimate
- Small: 1-2 weeks

### 9.2. Team size & composition
- Small Team: 1-2 total people
  - 1 Developer familiar with Python and web scraping
  - Optionally 1 SEO/content analyst for testing and validation

### 9.3. Suggested phases
- **Phase 1**: Core Framework Development (3-4 days)
  - Key deliverables: Repository structure, configuration system, sitemap fetching and parsing
- **Phase 2**: Data Processing and Storage (2-3 days)
  - Key deliverables: Change detection logic, Parquet data storage, basic reporting
- **Phase 3**: Automation and Testing (2-3 days)
  - Key deliverables: GitHub Actions workflow, error handling, documentation

## 10. User stories
### 10.1. Configure domains to monitor
- **ID**: US-001
- **Description**: As an administrator, I want to configure which competitor domains to monitor so that I can track relevant competitors.
- **Acceptance criteria**:
  - A config.json file exists that can be edited to add or modify domains
  - Each domain entry requires a name, domain, and sitemap URL
  - Changes to configuration are picked up on the next run
  - Invalid configuration entries result in clear error messages
  - The system validates that sitemap URLs are accessible

### 10.2. Fetch and parse sitemaps
- **ID**: US-002
- **Description**: As a user, I want the system to automatically fetch and parse competitor sitemaps so that I have current data about their URLs.
- **Acceptance criteria**:
  - System can fetch XML from specified sitemap URLs
  - System handles sitemap index files correctly
  - System extracts URL, lastmod, priority, and changefreq attributes when available
  - System properly handles different XML namespaces
  - System implements appropriate error handling for network issues

### 10.3. Detect new URLs
- **ID**: US-003
- **Description**: As an SEO specialist, I want to identify when competitors publish new URLs so that I can analyze their new content areas.
- **Acceptance criteria**:
  - System compares current sitemap URLs with previously stored URLs
  - New URLs are flagged and stored with "new" change type
  - System records timestamp when URL was first discovered
  - System groups new URLs by domain for easy filtering
  - New URLs are identified even if they appear in a different subsitemap than previously processed

### 10.4. Track URL modifications
- **ID**: US-004
- **Description**: As a content strategist, I want to know when competitors update existing content so that I can review their changes.
- **Acceptance criteria**:
  - System detects when lastmod date changes for existing URLs
  - Modified URLs are stored with "updated" change type
  - System records both old and new lastmod dates
  - System timestamps when the change was detected
  - Modified URLs are trackable over time to identify frequently updated content

### 10.5. Identify removed URLs
- **ID**: US-005
- **Description**: As a marketing analyst, I want to know when competitors remove URLs so that I can identify discontinued content areas.
- **Acceptance criteria**:
  - System detects when a previously tracked URL is no longer in the sitemap
  - Removed URLs are stored with "removed" change type
  - System maintains record of removed URLs with date of removal
  - System doesn't falsely flag URLs as removed during temporary sitemap issues
  - System can distinguish between URLs moved to different subsitemaps versus truly removed URLs

### 10.6. Run automated monitoring
- **ID**: US-006
- **Description**: As a user, I want the monitoring to run automatically each day so that I have up-to-date information without manual intervention.
- **Acceptance criteria**:
  - GitHub Actions workflow configured to run daily at a specified time
  - Workflow can also be triggered manually
  - Successful runs commit data changes back to the repository
  - Failed runs provide clear error logs
  - System sends notification on workflow completion (success or failure)

### 10.7. Access historical data
- **ID**: US-007
- **Description**: As an analyst, I want to access historical data about URL changes so that I can identify patterns over time.
- **Acceptance criteria**:
  - All URL changes are stored with timestamps
  - Data is stored in a format (Parquet) that allows for easy querying and filtering
  - Historical data for a URL shows its complete lifecycle (new, updates, removal)
  - System maintains a reasonable history period within storage constraints
  - Data can be exported or analyzed with standard tools like pandas

### 10.8. Secure access to data
- **ID**: US-008
- **Description**: As an administrator, I want to control who can access the monitoring data so that competitive intelligence remains secure.
- **Acceptance criteria**:
  - Repository can be set as private on GitHub
  - Access permissions can be managed through GitHub's user management
  - Sensitive configuration values can be stored as GitHub secrets
  - No sensitive credentials are committed to the repository
  - System doesn't expose competitor data publicly 