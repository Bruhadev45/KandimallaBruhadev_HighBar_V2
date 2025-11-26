# 🚀 Quick Start Guide

## Your Project is Ready!

### ✅ What's Set Up:
- ✓ Virtual environment created
- ✓ All dependencies installed (OpenAI, Streamlit, Plotly, etc.)
- ✓ OpenAI API key configured
- ✓ Streamlit frontend added
- ✓ Project converted from Anthropic to OpenAI

---

## 🎯 Two Ways to Use the Project

### Option 1: Web Interface (RECOMMENDED) 🌐

**The Streamlit app is currently RUNNING at:**
```
http://localhost:8501
```

**Just open that URL in your browser!**

**To restart it later:**
```bash
cd /Users/bruuu/Downloads/kasparro-agentic-fb-analyst-bruuu
source .venv/bin/activate
./start_frontend.sh
```

**What you can do:**
- Enter analysis queries (e.g., "Analyze ROAS drop in last 7 days")
- View interactive charts and visualizations
- See AI-generated insights with confidence scores
- Get creative recommendations
- Download reports as Markdown or JSON
- Explore 4,500 rows of Facebook Ads data

---

### Option 2: Command Line 💻

**Run analysis from terminal:**
```bash
cd /Users/bruuu/Downloads/kasparro-agentic-fb-analyst-bruuu
source .venv/bin/activate
python run.py "Your query here"
```

**Example:**
```bash
python run.py "Analyze ROAS drop in last 7 days"
```

**Output files:**
- `reports/report.md` - Analysis report
- `reports/insights.json` - Structured insights
- `reports/creatives.json` - Creative recommendations
- `logs/execution_*.json` - Execution logs

---

## 📊 What This Project Does

This is an **AI-powered multi-agent system** that:

1. **Analyzes** Facebook Ads performance data
2. **Identifies** issues causing ROAS drops or low CTR
3. **Generates** hypotheses about problems
4. **Validates** those hypotheses with evidence
5. **Recommends** specific creative improvements

**Uses 5 AI Agents:**
1. Planner - Breaks down your query
2. Data Agent - Analyzes the numbers
3. Insight Agent - Generates hypotheses
4. Evaluator - Validates with evidence
5. Creative Generator - Suggests improvements

---

## 🎨 Example Queries

Try these in the web interface or CLI:

```
"Analyze ROAS drop in last 7 days"
"Why did ROAS drop in the last week?"
"Which campaigns have low CTR and why?"
"Identify creative fatigue in our campaigns"
"Compare Facebook vs Instagram performance"
"Full performance audit: identify issues and recommend solutions"
```

---

## 📁 Key Files

```
app.py                  # Streamlit web interface ← Start here!
run.py                  # Command line interface
start_frontend.sh       # Quick launch script

requirements.txt        # All Python dependencies
.env                    # Your OpenAI API key (configured!)

config/config.yaml      # Model settings, thresholds
data/                   # Facebook Ads data (4,500 rows)
reports/                # Generated reports
logs/                   # Execution logs

FRONTEND_README.md      # Detailed frontend docs
STREAMLIT_FEATURES.md   # Feature summary
```

---

## 🔑 Current Configuration

**API:** OpenAI (GPT-4 Turbo)
**Model:** gpt-4-turbo-preview
**Temperature:** 0.7
**Max Tokens:** 4000
**Confidence Threshold:** 0.6

**Your API Key:** Configured in `.env` file ✓

---

## 🌐 Access URLs

**Local Access:**
```
http://localhost:8501
```

**Network Access (from other devices on same network):**
```
http://192.168.1.125:8501
```

---

## 💡 Tips

1. **First analysis takes ~2 minutes** - This is normal! The AI is:
   - Loading 4,500 rows of data
   - Making multiple LLM API calls
   - Running 5 different agents
   - Validating hypotheses

2. **Example queries work best** - Click them in the sidebar for quick results

3. **Explore the Data Overview tab** - See charts and metrics before running analysis

4. **Download reports** - Export results for presentations or sharing

5. **Try different queries** - The system adapts to any question about your ads

---

## 🆘 If Something's Wrong

**Web app not loading?**
```bash
# Stop and restart
pkill -f streamlit
./start_frontend.sh
```

**API key error?**
- Check `.env` file has: `OPENAI_API_KEY=your_key_here`
- Restart the app after updating

**Port 8501 already in use?**
```bash
streamlit run app.py --server.port 8502
```

**Python version issue?**
- Requires Python 3.9 or higher
- Check with: `python --version`

---

## 📚 Full Documentation

- **Frontend Guide:** `FRONTEND_README.md`
- **Features List:** `STREAMLIT_FEATURES.md`
- **Main README:** `README.md`
- **Architecture:** `ARCHITECTURE_VISUAL.md`

---

## 🎉 You're All Set!

**Go to:** http://localhost:8501

**Try it now:**
1. Click "Analyze ROAS drop in last 7 days" in the sidebar
2. Or enter your own query
3. Click the 🚀 Analyze button
4. Wait ~2 minutes for results
5. Explore the insights!

**Enjoy your AI-powered Facebook Ads analyst!** 🚀
