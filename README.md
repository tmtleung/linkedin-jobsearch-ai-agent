# jobsearch-automation-agent  
A modular job-search automation tool that scrapes job postings, filters results, removes duplicates, and generates a clean daily or weekly digest.  
Fully configurable, cross-platform, and beginner-friendly.

---

## Features

- Automatic job scraping (LinkedIn included by default)
- Keyword-based title and location filtering
- Posting-age extraction (“Posted X days ago”)
- Job deduplication using historical tracking
- Markdown digest generation
- Optional email notifications (Gmail App Password supported)
- Configuration-driven design — no code changes needed
- Extendable scraper architecture (add Greenhouse, Lever, Indeed, etc.)
- Works on macOS, Linux, and Windows
- Optional automation via launchd, cron, or Task Scheduler

---

## Getting Started

### 1. Clone the repository
```
git clone https://github.com/<your-username>/jobsearch-automation-agent.git
cd jobsearch-automation-agent
```

### 2. Run the setup script

**macOS / Linux**
```
./setup.sh
```

If needed:
```
chmod +x setup.sh
./setup.sh
```

**Windows**
```
powershell -ExecutionPolicy Bypass -File setup.ps1
```

This will:
- create a virtual environment  
- install dependencies  
- generate `config/config.json` from the template  

---

## Configuration

All settings live in:

```
config/config.json
```

It defines:

- title keywords  
- location keywords  
- maximum posting age  
- email settings  
- scraper options  

Example:

```json
{
    "scrapers": {
        "linkedin": {
            "enabled": true,
            "keywords": ["Software Engineer", "Data Analyst", "Intern"],
            "location_keywords": ["Remote", "United States"],
            "max_pages": 10
        }
    },
    "filters": {
        "max_post_age_days": 30
    },
    "email_notifications": {
        "enabled": false,
        "sender": "youremail@example.com",
        "recipients": ["youremail@example.com"],
        "smtp_host": "smtp.gmail.com",
        "smtp_port": 587,
        "use_tls": true,
        "subject_template": "Job Digest — {{date}}"
    }
}
```

---

## Customizing Your Location Search

Modify:

```
"location_keywords": [...]
```

### Examples

**Search a city:**
```json
["San Francisco"]
```

**Search multiple cities:**
```json
["Austin", "Seattle", "Denver"]
```

**Search remote-only:**
```json
["Remote"]
```

**Remove location filtering:**
```json
[]
```

### Why no radius setting?

Public job listings do not expose a reliable radius parameter.  
Keyword-based location filtering works globally across all platforms.

---

## Running the Agent

```
python run_agent.py
```

The agent will:

1. Run enabled scrapers  
2. Filter by title, location, posting age  
3. Remove duplicates using `history/job_history.json`  
4. Generate a digest in `outputs/latest_digest.md`  
5. Optionally send it via email  

---

## Email Digest (Optional)

Requires a Gmail App Password.

### macOS / Linux:
```
export EMAIL_PASSWORD="your-app-password"
```

### Windows:
```
setx EMAIL_PASSWORD "your-app-password"
```

Enable in config:

```json
"email_notifications": { "enabled": true }
```

---

## Automation

Automation guides are in:

```
docs/automation.md
```

Includes:

- macOS launchd  
- Linux cron  
- Windows Task Scheduler  

---

## Extending the Agent

Add scrapers in:

```
scrapers/
```

New scrapers must return jobs in this format:

```python
{
    "title": str,
    "company": str,
    "location": str,
    "url": str,
    "source": str
}
```

---

## Project Structure

```
jobsearch-automation-agent/
│
├── config/
│   ├── config.example.json
│   └── config.json
│
├── scrapers/
│   ├── linkedin_scraper.py
│   └── example_scraper_template.py
│
├── processor/
│   ├── filters.py
│   ├── relevance_classifier.py
│   └── dedupe.py
│
├── output/
│   ├── formatter.py
│   └── digest_builder.py
│
├── emailer/
│   └── mailer.py
│
├── utils/
│   ├── logger.py
│   └── html_utils.py
│
├── history/
│   └── .gitkeep
│
├── logs/
│   └── .gitkeep
│
├── outputs/
│   └── .gitkeep
│
├── docs/
│   ├── automation.md
│   └── filters.md
│
├── run_agent.py
├── setup.sh
├── setup.ps1
├── requirements.txt
└── LICENSE
```

---

## Future Enhancements

- AI-powered job relevance scoring  
- Resume screening  
- Automated resume tailoring suggestions  
- AI-generated job summaries  
- Additional scraper modules (Greenhouse, Lever, Indeed)  
- Enhanced date and location parsing  

These would be optional modules requiring user-provided API keys.

---

## License
MIT License.

---

## Contributing
Pull requests are welcome.

