# Suricata - Demo

## What is Suricata?

Suricata is a free and open-source network threat detection engine developed by the Open Information Security Foundation (OISF). It functions as both an Intrusion Detection System (IDS) and an Intrusion Prevention System (IPS), making it a powerful tool for monitoring and securing network environments.

## What will be achieved in this demo?

This Suricata demo showcases how custom IDS (Intrusion Detection System) rules can be used to monitor and detect specific network activities. The demo will:

- **Detect Access to a Specific IP Address** - Alerts will be generated when any IP communication targets the suspicious IP address 194.242.57.111, potentially associated with malicious domains like leomalay.com.
- **Identify DNS Queries for Popular Domains** - Custom rules will flag DNS queries containing the keywords “google” and “facebook,” demonstrating domain-based monitoring.
- **Detect PDF File Transfers over HTTP** - The demo will detect and log incoming HTTP traffic containing PDF files, using file signature (magic number) inspection, and store those files for further analysis.

Overall, this demo will illustrate Suricata’s ability to perform deep packet inspection, identify application-layer activity (DNS, HTTP), and help detect early signs of threat activity using tailored rule sets.

## Steps to perform

> > > WARNING: Never run this on production server or your PC. This demo invloves use of SUDO which is dangerous. Perform at your own risk.
> > > Here are the steps to be performed with some commads, but I recommed you to visit [https://notes.leomalay.com/#/tools/suricata](https://notes.leomalay.com/#/tools/suricata)

1. Open Kali-Linux VM or any other Linux based system.
2. Install suricata on this VM.
3. Copy the provided `rules` file in this repository at location `/var/lib/suricata/rules/`.
4. Editing `/etc/suricata/suricata.yaml` config file at 2 locations.
   - Add rules file `local_test.rules` under `rule-files` section.
   - Enable `filestore` under `file-store` section.
5. Test your configuations using the following command.

   ```bash
   ($) sudo suricata -T -c /etc/suricata/suricata.yaml -v
   ```

6. Find out your network interface using the following command. It should be `eth0`, `wlan0`, etc.

   ```bash
   ($) ip addr
   ```

7. Use the following command to start the suricata in the backgound. Don't forget to replace your network interface.

   ```bash
   ($) sudo suricata -c /etc/suricata/suricata.yaml -i eth0 &
   ```

   Please wait for **1 minute** for it to start completely.

8. Now perform the following commads to tribber alerts.

   ```bash
   ($) curl -k https://194.242.57.111
   ($) host www.google.com
   ($) host www.facebook.com
   ($) wget http://s24.q4cdn.com/216390268/files/doc_downloads/test.pdf
   ```

   Please note that filestore will **_NOT_** work for `HTTPS`

9. Kill the suricata process running in the background.

   ```bash
   ($) sudo kill <pid>
   ```

10. Check the logs using the following command.

    ```bash
    ($) cat /var/log/suricata/fast.log
    ```

## Output

Upon completing the above steps, you will observe alert outputs similar to those shown in the fast.log file included in the repository. This file captures Suricata’s real-time detection alerts, providing a clear view of how the custom rules are triggered during the demo.

## Now What?

Thanks for following along with the demo! 🎉

Now that you've seen how Suricata works with custom rules and logging, you're ready to dive deeper. Feel free to:

- 🛠️ Create your own custom rules to detect specific traffic or behaviors relevant to your environment.
- ⚙️ Experiment with Suricata’s configuration settings to tailor detection, performance, and logging to your needs.
- 🔍 Analyze the logs and extracted files to better understand how Suricata interprets and flags network traffic.

This is just the beginning — there's a lot more to explore in network security and threat detection. Keep learning, keep experimenting, and happy packet hunting! 🚀
