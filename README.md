# Solix DePIN Automation 🚀

A fully asynchronous Python 3.11+ tool to automate interactions with the Solix/DePIN Stork API. Supports bulk registration, farming (connection quality), statistics updates, task automation, and CSV export.

## 📑 Table of Contents
- [Features](#features-✨)
- [Prerequisites](#prerequisites-📋)
- [Installation](#installation-⚙️)
- [Configuration](#configuration-📝)
- [Data Files](#data-files-📂)
- [Usage](#usage-🚀)
  - [Interactive Menu](#interactive-menu)
  - [Standalone Modes](#standalone-modes)
- [Modes](#modes-🛠️)
- [Logging](#logging-📜)
- [Proxy & Captcha](#proxy--captcha-🌐🔒)
- [Database](#database-🗄️)
- [Contributing](#contributing-🤝)
- [License](#license-📄)

---

## Features ✨
- ✅ **Async-first**: Built with `asyncio` & `curl_cffi` for high concurrency
- 📝 **Bulk Registration**: AWS Cognito–style endpoints with Turnstile captcha solving
- 🌐 **Farming**: Periodic connection quality reporting
- 📊 **Statistics Update**: Fetch total/task/referral points
- 🤖 **Task Automation**: Follow/like/claim social tasks automatically
- 💾 **CSV Export**: Quick export via `pandas`
- 🔄 **Round-robin Proxies**: Per-account proxy assignment
- 🔒 **Captcha Solving**: CapSolver integration (Turnstile & HCaptcha)
- 🗄️ **SQLite DB**: Local storage with `aiosqlite`
- 🌈 **Rich CLI**: Interactive menu powered by `rich`
- 📜 **Detailed Logs**: `loguru` with custom `SUCCESS` level

## Prerequisites 📋
- Python 3.11 or newer
- `libcurl` installed (for `curl_cffi`)
- A valid CapSolver API key
- Input files in `data/`: `registration.txt`, `farming.txt`, `proxy.txt`

## Installation ⚙️
1. **Clone repository**:
   ```bash
   git clone https://github.com/yourusername/solix-automation.git
   cd solix-automation
   ```
2. **Create virtual environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   ```
3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Configuration 📝
Edit `settings.yaml` to match your environment:
```yaml
logging:
  level: INFO
  file: logs/app.log

api:
  base_url: https://api.solixdepin.net/api
  endpoints:
    register: /auth/register
    login: /auth/login-password
    stats: /point/get-total-point
    tasks: /task/get-user-task
    do_task: /task/do-task
    claim_task: /task/claim-task
    connection_quality: /point/get-connection-quality

database:
  path: database/solix.db

captcha:
  provider: capsolver
  api_key: 'YOUR_CAPSOLVER_API_KEY'
  turnstile_site_key: 'YOUR_TURNSTILE_SITE_KEY'
  turnstile_page_url: 'https://dashboard.solixdepin.net/'
  create_task_url: 'https://api.capsolver.com/createTask'
  get_result_url: 'https://api.capsolver.com/getTaskResult'

proxy:
  file: data/proxy.txt
  rotate_on_error: true

registration:
  concurrency: 5
  initial_delay_seconds: 1
  referral_code: ''

farming:
  concurrency: 5
  initial_delay_seconds: 1
  interval_seconds: 60

update_stats:
  concurrency: 5
  initial_delay_seconds: 1

tasks:
  concurrency: 5
  execution_delay_seconds: 10

export:
  output_file: data/stats_export.csv

data_files:
  registration_list: data/registration.txt
  farming_list: data/farming.txt
```

## Data Files 📂
- **`data/registration.txt`** & **`data/farming.txt`**: one `email:password` per line
- **`data/proxy.txt`**: one `user:pass@host:port` proxy per line

## Usage 🚀
### Interactive Menu
```bash
python main.py
```
- Navigate with ↑/↓, press **Enter** to select.
- Modes: Registration, Farming, Update Stats, Execute Tasks, Export Stats, Exit.

### Standalone Modes
```bash
python core/registration.py
python core/farming.py
python core/update_stats.py
python core/tasks.py
python core/export_stats.py
```

## Modes 🛠️
- **Registration**: Bulk signup + token storage
- **Farming**: Continuous connection quality polling
- **Update Statistics**: One‑time total/task/referral point update
- **Execute Tasks**: Perform & claim social tasks
- **Export Statistics**: Dump DB to CSV (`data/stats_export.csv`)

## Logging 📜
- Uses `loguru` with levels: DEBUG, INFO, SUCCESS, ERROR
- Logs to console + file (`logs/app.log`)
- Key events (delays, successes, errors) are annotated with account email

## Proxy & Captcha 🌐🔒
- **Round‑Robin Proxies**: consistent assignment per account email
- **Captcha Solving**: CapSolver API with `AntiTurnstileTaskProxyLess` fallback

## Database 🗄️
- SQLite file at `database/solix.db` with table `accounts`
- Fields: email, password, tokens, referral code, stats, connection quality

## Contributing 🤝
Feel free to open issues or pull requests. All contributions welcome! 👍

## License 📄
MIT © Your Name 