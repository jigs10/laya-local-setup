# Laya Local Setup & Test Commands

Commands and examples used in my video for running **Laya locally**, testing it with different scenarios, and comparing it with Jev.

## 1. Clone the Repository

```bash
git clone https://github.com/NandhaKishorM/laya.git
cd laya
```

## 2. Install Laya

```bash
pip install "laya[serve]"
```

## 3. Start the Demo Server

```bash
python examples/server.py
```

This starts the local web interface and downloads the required Laya models.

---

# Test Examples

## Example 1: Scam Message

### Input

```json
{
  "text": "Your Netflix account will be locked in 2 hours. Confirm your password here: http://netflix-login-help.xyz"
}
```

### Questions

```json
{
  "what_is_this": {
    "type": "choice",
    "instructions": "What is this message?",
    "criteria": {
      "scam": "fake warning to steal a password",
      "real": "real Netflix message",
      "ads": "normal marketing",
      "other": "none of those"
    }
  },
  "should_click": {
    "type": "noul",
    "instructions": "Should the person click the link?"
  },
  "danger": {
    "type": "score",
    "instructions": "How dangerous is this?",
    "criteria": [
      "harmless",
      "suspicious",
      "dangerous"
    ]
  }
}
```

## Example 2: Weather

### Input

```json
{
  "forecast": "Heavy rain all afternoon. Thunderstorms after 3pm. Wind 30 km/h."
}
```

### Questions

```json
{
  "bring": {
    "type": "choice",
    "instructions": "What should they take outside?",
    "criteria": {
      "umbrella": "rain expected",
      "jacket": "cold but dry",
      "nothing": "fine as is",
      "stay_home": "too bad to go out"
    }
  },
  "will_rain": {
    "type": "noul",
    "instructions": "Will it rain today?"
  }
}
```

## Example 3: Restaurant Review

### Input

```json
{
  "review": "Pasta was cold, waiter never came back, they added a 20% tip we did not agree to. Never again."
}
```

### Questions

```json
{
  "sentiment": {
    "type": "choice",
    "instructions": "Is this review positive or negative?",
    "criteria": {
      "positive": "they liked it",
      "negative": "they did not like it",
      "mixed": "both good and bad"
    }
  },
  "complaint": {
    "type": "noul",
    "instructions": "Is this a complaint?"
  },
  "how_bad": {
    "type": "score",
    "instructions": "How unhappy is the customer?",
    "criteria": [
      "mild",
      "upset",
      "furious"
    ]
  }
}
```

## Example 4: Job Offer

### Input

```json
{
  "offer": "Junior role, $95k, remote, 25 days vacation, good team. Commute would have been 2 hours."
}
```

### Questions

```json
{
  "decision": {
    "type": "choice",
    "instructions": "Should they take the job?",
    "criteria": {
      "accept": "good offer",
      "negotiate": "ok but ask for more",
      "reject": "not worth it"
    }
  },
  "fair_pay": {
    "type": "noul",
    "instructions": "Does this sound like fair pay for a junior remote job?"
  }
}
```

## Example 5: Misleading Headline

### Input

```json
{
  "headline": "Scientists say drinking 12 cups of coffee a day reverses aging, study of 8 people finds"
}
```

### Questions

```json
{
  "trust": {
    "type": "choice",
    "instructions": "How trustworthy is this headline?",
    "criteria": {
      "solid": "credible science",
      "weak": "overhyped",
      "junk": "clickbait or fake"
    }
  },
  "share": {
    "type": "noul",
    "instructions": "Should someone share this as fact?"
  }
}
```

---

# Uninstall Laya

## 1. Uninstall Laya

```bash
pip uninstall -y laya
```

## 2. Check if Laya Is Uninstalled

```bash
pip show laya
```

## 3. Delete Downloaded Models

Run these commands in **PowerShell**:

```powershell
$hub = Join-Path $env:USERPROFILE ".cache\huggingface\hub"

Remove-Item -Recurse -Force -ErrorAction SilentlyContinue "$hub\models--convaiinnovations--laya"
Remove-Item -Recurse -Force -ErrorAction SilentlyContinue "$hub\models--convaiinnovations--laya-multilingual"
Remove-Item -Recurse -Force -ErrorAction SilentlyContinue "$hub\models--convaiinnovations--laya-typed-decisions"
Remove-Item -Recurse -Force -ErrorAction SilentlyContinue "$env:USERPROFILE\.cache\receptron-laya"
Remove-Item -Recurse -Force -ErrorAction SilentlyContinue "$env:USERPROFILE\.laya"
```

## 4. Final Check

```bash
pip show laya
```

Then check the Hugging Face cache:

```powershell
$hub = Join-Path $env:USERPROFILE ".cache\huggingface\hub"

if (Test-Path $hub) {
  Get-ChildItem $hub -Directory | Where-Object { $_.Name -match "laya" }
} else {
  Write-Host "no huggingface cache folder"
}
```

If `pip show laya` returns nothing and no Laya model folders are listed, Laya and its downloaded model files have been removed.
