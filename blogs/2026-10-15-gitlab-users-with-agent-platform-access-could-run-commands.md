---
title: "GitLab Users With Agent Platform Access Could Run Commands on Its Self-Hosted AI Gateway"
url: "https://www.cakewalk.security/blog/gitlab-ai-gateway-cve-2026-90970"
date: "2026-10-15"
author: "Johannes Keienburg"
feed_url: "https://www.cakewalk.security/rss.xml"
---
GitLab fixed CVE-2026-90970 on October 2, 2026. A signed-in user with access to its Duo Agent Platform could use a crafted flow configuration to run arbitrary commands on a self-hosted AI Gateway, the server that issues the signed tokens authorizing GitLab Duo requests. GitLab rates it 9.9.
