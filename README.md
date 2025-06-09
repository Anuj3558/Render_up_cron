Here is a detailed `README.md` file for your **Scheduled GET Request** GitHub Action that helps keep a Render-hosted backend awake by periodically pinging it.

---

# 🕒 Scheduled GET Request – Keep Render Services Awake

This GitHub Action periodically sends a GET request to a specified URL. It is particularly useful for preventing **Render's free-tier web services** from going into sleep mode due to inactivity. By hitting the endpoint every minute, your Render service stays "warm" and responsive.

---

## 🚀 Purpose

Render’s free-tier services automatically go to sleep after 15 minutes of inactivity. When this happens, the next request takes longer to process while the service "wakes up." This GitHub Action is a **keep-alive mechanism** that regularly pings your backend endpoint to avoid that delay.

---

## ⚙️ How It Works

This workflow is triggered every **minute** using GitHub's `schedule` event and the cron syntax. Each run executes a `curl` command to your provided URL.

```yaml
name: Scheduled GET Request

on:
  schedule:
    - cron: '*/1 * * * *'  # Every 1 minute

jobs:
  ping-url:
    runs-on: ubuntu-latest

    steps:
      - name: Hit GET Request URL
        run: |
          RESPONSE=$(curl -s https://student-result-management-system-bvv6.onrender.com/api/result-formats)
          echo "Response: $RESPONSE"
```

---

## ✅ Benefits

* 🔄 **Keeps your Render backend active**
* 🆓 **Utilizes GitHub Actions free minutes (up to 2,000 mins/month on free tier)**
* ⚡ **Reduces first-request latency after inactivity**
* 💰 **No need for paid uptime solutions like UptimeRobot or CronJobs**

---

## 🛠️ Setup Instructions

1. **Go to your GitHub repository**.
2. **Create a new file** under `.github/workflows/keep-alive.yml` or any name you prefer.
3. **Paste the above YAML code** into that file.
4. Commit and push to your repository.
5. GitHub Actions will now automatically run the workflow every minute.

---

## 🔄 Customize It

* **Change the schedule**: Adjust the cron timing to every 5 minutes (as originally commented) or as per your requirement.

  ```yaml
  - cron: '*/5 * * * *'  # Every 5 minutes
  ```
* **Ping multiple endpoints**: Add more `curl` commands under the `run` section.
* **Add a manual trigger**:

  ```yaml
  on:
    workflow_dispatch:
    schedule:
      - cron: '*/1 * * * *'
  ```

---

## 🧾 Notes

* GitHub Actions allows **2,000 free minutes/month** per account for public repositories.
* A workflow triggered every minute consumes \~43,200 minutes per month (1 min × 60 mins × 24 hrs × 30 days), so this strategy is best for **short jobs** and when paired with **manual stop conditions**.
* To save usage, consider setting it to **every 5 or 10 minutes** if less frequent wake-ups are acceptable.

---

## 📌 Example Use Case

You're hosting an API on Render:

`https://student-result-management-system-bvv6.onrender.com/api/result-formats`

Render puts it to sleep after 15 minutes of no activity. This GitHub Action ensures it's pinged regularly to keep it ready for any student or admin querying the result data—offering a **snappier user experience**.

---

## 💡 Tip

To track successful hits, consider logging timestamps or using a service like [Loggly](https://www.loggly.com/) or self-hosted logs to verify uptime.

---

## 📚 Resources

* [Render Free Web Services](https://render.com/docs/free#web-services)
* [GitHub Actions Cron Syntax](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule)
* [curl Manual](https://curl.se/docs/manpage.html)

---

Feel free to copy and edit this README for your specific usage. Let me know if you'd like it in `.md` file format.
