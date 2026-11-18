# Notes from a first time customer

I went through your CoreDNS setup as a brand new user, on a local Kubernetes cluster. Most of it just worked. The install took about a minute, the dashboard showed my server straight away, and the alert reached my inbox a few minutes after I saved it. The plain English sentence at the bottom of the trigger form is a lovely touch.

Three things slowed me down. Each one comes with a fix you could use.

## 1. The Kubernetes route is not written down yet

**What happened.** I read the CoreDNS docs page and the knowledge base article. The article says to run Xitogent as a DaemonSet but does not show how. The docs page says to enable the integration on the server where CoreDNS is installed, and it expects the metrics at localhost. In my cluster CoreDNS runs in a pod, so the dashboard said the integration was not detected and the Enable button stayed greyed out.

What fixed it was the command line. `xitogent integrate` asks for a metrics host and port, and when I gave it the kube-dns service address, everything worked. I could not find that written anywhere. I also mistyped the address by one digit, and the error only said it failed to connect, without saying which address it had tried.

**Suggested fix.**
- Add a short Kubernetes section to the CoreDNS docs page: find the service address with one `kubectl` command, check it with `curl`, then enter it in `xitogent integrate`.
- Include the address in the connection error.
- If a DaemonSet is the route you recommend, a ready made manifest or image would help most.

I only tested inside a kind node, so please check the steps against how you would really deploy it. I would be glad to write that section.

## 2. Two docs sentences that no longer match the product

**Creating triggers.** The Server Monitoring page says you can create and edit triggers from the Server overview page. On 17 November (dashboard v3.3) I could not find the control there, because the overview only has a summary box. I eventually found a small plus icon on the Triggers tab. By 18 November (v3.4) that tab has an Add trigger button with a CoreDNS option, so the product has already improved.

*Fix:* change that sentence to point to the Triggers tab and the Add trigger button, and add one screenshot.

**The status command.** The docs have a page for `xitogent status`, last updated in April 2025. The Xitogent CLI overview does not list it, and my agent (version 2.13) said the command does not exist.

*Fix:* either add it to the CLI list with the version or platform it belongs to, or retire the page and point readers to whatever replaced it.

## 3. The "agent stopped reporting" email includes the agent's credentials

My laptop lost power and wifi overnight, and Xitoring emailed me. I loved that, because it was a real alert I did not have to stage. But the email body contained the server's registration key, ID and password in plain text, next to a one line fix command. Emails get forwarded, screenshotted and kept for years.

**Suggested fix.** Link to the server page in the dashboard, where the fix command can sit behind a login, or switch the fix command to a short lived token.

---

I left out smaller things, like an empty popup message and a few graphs that stay flat on a quiet cluster, because they felt like polish rather than blockers. Happy to share them if useful.
