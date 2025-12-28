# gcp


## Integrate Google Cloud Dataform with GitHub
To integrate Google Cloud Dataform with GitHub, you need to create a Personal Access Token (PAT). GitHub offers two types of tokens: **Fine-grained** (recommended for security) and **Classic**.

Here are the specific permissions you need to select for each:

### Option 1: Fine-grained Token (Recommended)

This is the most secure method because it limits the token's power to only the repository you choose.

1. **Repository Access:** Select **"Only select repositories"** and pick your Dataform repo.
2. **Permissions:** Scroll down to **"Repository permissions"** and set the following:
* **Contents:** `Read and Write` (Required for committing code and pulling changes).
* **Metadata:** `Read-only` (Usually automatically selected; required for basic API access).


3. **Optional:** If you plan to use GitHub Actions to trigger Dataform later, you might also need **"Workflows: Read and Write"**.

### Option 2: Classic Token

If you are having trouble with fine-grained tokens (some older setups prefer Classic), use these settings:

1. **Scopes:** Check the top-level **`repo`** checkbox.
* This automatically includes sub-permissions like `repo:status`, `repo_deployment`, `public_repo`, etc.


2. **Optional:** If you are managing GitHub Actions through Dataform, also check the **`workflow`** scope.

---

### Important Next Steps in Google Cloud

Once you have copied your token from GitHub, don't forget these two critical steps in GCP to make the connection work:

1. **Store in Secret Manager:** You cannot paste the token directly into Dataform. You must create a new secret in **Secret Manager**, paste the token as the "Secret Value," and name it something like `dataform-github-token`.
2. **Grant Permissions to the Service Agent:** Dataform uses a special service account to "talk" to Secret Manager.
* Find your Dataform Service Agent (it usually looks like: `service-[PROJECT_NUMBER]@gcp-sa-dataform.iam.gserviceaccount.com`).
* Go to IAM and grant that agent the **"Secret Manager Secret Accessor"** role.



### Common Pitfall: SAML SSO

If your GitHub repository belongs to an **Organization** that uses Single Sign-On (SAML SSO), you must click the **"Configure SSO"** dropdown next to your token after generating it and click **"Authorize"**. If you skip this, Dataform will give you a "403 Forbidden" or "Repository not found" error even if the token is correct.

**Would you like me to show you the command to grant the Secret Manager role to your Dataform service account via the terminal?**