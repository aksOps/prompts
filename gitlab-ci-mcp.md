I need to build an **Air-Gapped GitLab CI Architect MCP Server** hosted on **AKS** within a private network.

Please act as a Principal Engineer. Generate the complete solution using **Fastify**, **TypeScript**, **better-sqlite3**, and **Docker**.

### **1. Core Architecture (Fastify on AKS)**
* **Framework:** Fastify v5.
* **Transport:** `SSEServerTransport` (Endpoints: `/sse`, `/messages`).
* **Deployment:** Dockerfile (Multi-stage) + Kubernetes Manifests.
* **Network Constraint:** The build environment has NO access to the public internet, but HAS access to an internal GitLab Enterprise instance.

### **2. Intelligence Modules (The Tools)**
Implement these specific tools in `src/tools.ts`:

#### **Tool A: `analyze_pipeline_maturity`**
* **Input:** Raw `.gitlab-ci.yml` content.
* **Logic:** Score the pipeline on "Modern Features" (DAG/needs, rules, cache, interruptible).
* **Output:** JSON report of optimizations.

#### **Tool B: `suggest_component_refactor`**
* **Input:** Raw `.gitlab-ci.yml` content.
* **Logic:** Identify repetitive script blocks (>3 lines match) and suggest a `spec:inputs` Component structure.

#### **Tool C: `get_knowledge_base`**
* **Input:** Search query or URL.
* **Logic:**
    * Check SQLite DB (`data/gitlab_docs.db`) first.
    * **Fallback (Internal Only):** If missing, fetch from the **Internal GitLab Instance** (e.g., `https://gitlab.internal.com/help/...`).
    * **Auth:** If the internal instance requires auth for `/help`, support a `GITLAB_TOKEN` env var.

### **3. Seeding Logic (The "Air-Gap" Fix)**
In `src/seed.ts`, do NOT use public URLs. Instead:
1.  Accept an environment variable `GITLAB_INSTANCE_URL` (default: `https://gitlab.example.com`).
2.  Construct target URLs dynamically targeting the **internal help pages**.
    * *Example Mapping:* Public `ci/yaml/` usually exists at `${GITLAB_INSTANCE_URL}/help/ci/yaml/index.md`.
3.  **Target List:**
    * `/help/ci/yaml/index.md` (Keywords)
    * `/help/ci/components/index.md` (Components)
    * `/help/ci/jobs/job_control.md` (Parallel/Matrix)
    * `/help/ci/caching/index.md` (Caching)
    * `/help/ci/pipelines/settings.md` (Interruptible)

### **4. Docker & Build**
* **Dockerfile:**
    * **Stage 1 (Builder):** Compile TS.
    * **Stage 2 (Seeder):**
        * ARG: `GITLAB_INSTANCE_URL`
        * ARG: `GITLAB_TOKEN` (Optional, for fetching docs)
        * RUN: `node dist/seed.js` (Fetches from internal GitLab and populates SQLite).
    * **Stage 3 (Runner):** Alpine image with the pre-seeded `data/gitlab_docs.db`.

### **Output Requirements**
Provide the full code for:
1.  `package.json`
2.  `src/index.ts` (Fastify Server)
3.  `src/tools.ts`
4.  `src/db.ts`
5.  `src/seed.ts` (The internal-facing fetcher)
6.  `Dockerfile` (With ARGs for internal URL)
7.  `k8s/manifests.yaml`
