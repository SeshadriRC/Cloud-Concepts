
**Datadog Spike Reasons**

- We observed a Datadog spike originating from DFS Secret Manager version 3.1.5. It was not properly fetching credentials from Vault for the application. To resolve the issue, we instructed the user to use DFS Secret Manager version 3.1.2. After the downgrade, the issue was resolved and no further spikes were observed. 
