# Create a Simple RAG System with AnythingLLM and LMStudio
*Author: Brody Kilpatrick*
## Summary

This beginner-friendly tutorial guides you through building a local Retrieval-Augmented Generation (RAG) system without writing code. You'll learn to set up your own AI assistant that can answer questions based on your documents, running entirely on your own hardware for complete privacy and control.

**What is RAG?** RAG combines document retrieval with AI language models, allowing the AI to reference your specific documents when answering questions, rather than relying solely on its training data.

**Time Required:** 1-2 hours for initial setup  
**Skill Level:** Beginner (no coding required)

---

## Hardware Requirements

### Recommended Configuration
*This setup provides good performance for single-user environments with reasonably fast response times.*

- **Operating System:** Linux Virtual Machine (Ubuntu 22.04 LTS or newer recommended)
- **CPU:** 16 cores (more cores = better performance; the system will utilize what's available)
- **GPU:** 4GB+ VRAM (GeForce GTX 1050 Ti is the absolute minimum; RTX 3060 or better recommended)
- **RAM:** 32GB system memory
- **Storage:** 40GB+ free space on NVMe SSD or SATA SSD

**Note:** This configuration is suitable for personal use or small team testing. For production environments with multiple concurrent users, significantly more GPU power is required.

### Minimum Configuration
*This setup will work but expect slower response times. Acceptable for proof-of-concept testing.*

- **Operating System:** Linux Virtual Machine (Ubuntu 22.04 LTS or newer)
  - Windows is supported but typically offers slower performance
- **RAM:** 16GB system memory (absolute minimum)
- **Storage:** 40GB+ free space on NVMe SSD or SATA SSD (HDD not recommended)

**Performance Note:** Without a GPU, the system will use CPU-only inference, which can be 10-20x slower depending on model size.

---

## Software Components

- **Ubuntu Desktop** (22.04 LTS or newer)
- **LMStudio** - Local LLM runtime and model management
- **AnythingLLM** - RAG interface and document vectorization platform

---

## Installation Instructions

### Step 1: Install Ubuntu

1. Download Ubuntu Desktop 22.04 LTS or newer from [ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)
2. Install Ubuntu using the standard installation wizard (default options are fine)
3. After installation completes, open Terminal and update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

4. Reboot your system to ensure all updates are applied:

```bash
sudo reboot
```

---

### Step 2: Install LMStudio

LMStudio provides a user-friendly interface for downloading, managing, and running local language models.

1. Visit [lmstudio.ai](https://lmstudio.ai/) and download the Linux version
2. Open Terminal and configure system permissions for LMStudio's sandbox environment:

```bash
# Enable unprivileged user namespaces (addresses kernel-level permission requirements)
echo "kernel.unprivileged_userns_clone = 1" | sudo tee /etc/sysctl.d/00-local-userns.conf

# Apply the new system configuration
sudo sysctl --system

# Configure AppArmor to allow unprivileged user namespaces
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
```

3. Navigate to your Downloads folder, right-click the LMStudio AppImage file, and select **"Properties"**
4. Go to the **"Permissions"** tab and check **"Allow executing file as program"**
5. Double-click the file to launch LMStudio

**First Launch Configuration:**
- Click **"Get Started"**
- Select your experience level (recommend **"Power User"** for more control)
- Click **"Continue"**

---

### Step 3: Install AnythingLLM

AnythingLLM provides the RAG interface that connects your documents to the language model.

**Official documentation:** [docs.anythingllm.com/installation-desktop/linux](https://docs.anythingllm.com/installation-desktop/linux)

Open Terminal and run the following commands:

```bash
# Install FUSE (required for AppImage support)
sudo apt install libfuse2 -y

# Install cURL (for downloading the installer)
sudo apt install curl -y

# Download the AnythingLLM installer script
curl -fsSL https://cdn.anythingllm.com/latest/installer.sh -o installer.sh

# Make the script executable
chmod +x installer.sh

# Run the installer
./installer.sh
```

Follow the on-screen prompts to complete the installation. The application will typically install to your home directory.

---

## Configuration

### Step 4: Configure LMStudio

**Download and Load Your First Model:**

1. Open LMStudio
2. Click the **Search icon** (magnifying glass) in the left sidebar
3. Search for and download one of these recommended starter models:
   - **qwen2.5-7b-instruct** (good balance of speed and quality)
   - **granite-3-8b-instruct** (excellent for RAG tasks)
   - **llama-3.2-3b-instruct** (fastest, lower resource usage)

4. Once downloaded, click the **💬 Chat** tab in the left sidebar
5. Select your model from the dropdown at the top
6. Test the model with a simple question like "What is the capital of France?"

**Start the LMStudio API Server:**

1. Click the **Developer** tab (🔌) in the left sidebar
2. Find the status indicator that says **"Status: Stopped"**
3. Click the **"Start Server"** button
4. Verify the server shows **"Status: Running"** and displays a local URL (typically `http://localhost:1234`)

**Important:** Keep LMStudio running with the server active whenever you're using AnythingLLM.

---

### Step 5: Configure AnythingLLM

**Initial Setup:**

1. Launch AnythingLLM Desktop (check your home directory or applications menu)
2. Click **"Get Started"**
3. On the **"LLM Preference"** screen:
   - Scroll down and select **"LMStudio"**
   - Choose the model you loaded in LMStudio (e.g., "qwen2.5-7b-instruct")
4. Click **"Next"** through the remaining setup screens (embedding and vector database defaults are fine)
5. Create your first workspace:
   - Name it something descriptive like **"Test Workspace"** or **"My Documents"**
   - Click **"Create Workspace"**

**Test Basic Functionality:**

Send a test message like "Hello, can you introduce yourself?" to verify the connection is working.

---

### Step 6: Vectorize Your First Document

This is where the RAG magic happens—you'll teach your AI about specific documents.

**Upload and Embed a Document:**

1. In your workspace, click the **Upload icon** (up arrow) in the bottom-left corner
2. Click **"Upload Files"** and select a well-formatted document:
   - **Best formats:** PDF, TXT, DOCX, Markdown
   - **Best content:** Documents with clear structure, headings, and relevant information
   - **Size:** Start with smaller documents (under 100 pages) for faster processing
3. Once uploaded, you'll see your document in the file list
4. Select the document by checking its box
5. Click **"Move to Workspace"**
6. Click **"Save and Embed"**

**What's Happening?** AnythingLLM is breaking your document into chunks, converting them into mathematical representations (vectors), and storing them in a searchable database.

**Test Your RAG System:**

1. Go back to your workspace chat
2. Start a new thread (click the **+** icon)
3. Ask questions about your document:
   - "What documents do you have access to?"
   - "Summarize the main points from [document name]"
   - "What does the document say about [specific topic]?"

**Pro Tips:**
- Use specific keywords that appear in your document for best results
- The AI will cite which parts of the document it's referencing
- If answers seem off, try rephrasing your question or being more specific

---

## Troubleshooting

### Common Issues and Solutions

**LMStudio server won't start:**
- Check if another application is using port 1234
- Restart LMStudio completely
- Verify the model is fully downloaded (check the Downloads section)

**AnythingLLM can't connect to LMStudio:**
- Ensure LMStudio's server status shows "Running"
- Verify the model is loaded in LMStudio's chat interface
- Try restarting both applications

**Document embedding is slow or failing:**
- Start with smaller documents (under 10MB)
- Ensure you have sufficient RAM available
- Check that the document format is supported

**Answers don't reference the document:**
- Verify the document embedded successfully (check workspace settings)
- Use keywords that definitely appear in your document
- Try asking more specific questions
- Some models perform better than others for RAG—experiment with different models

---

## Next Steps

**You've completed the basic setup!** Here are ways to improve your RAG system:

### Optimization Tips

1. **Try Different Models:**
   - Larger models (13B+) provide better reasoning but are slower
   - Experiment with models specifically fine-tuned for RAG tasks
   - Browse [lmstudio.ai/models](https://lmstudio.ai/models) for options

2. **Improve Document Quality:**
   - Use well-formatted documents with clear headings
   - Break large documents into logical sections
   - Remove unnecessary formatting or images that don't add value

3. **Tune Embedding Settings:**
   - In AnythingLLM settings, adjust chunk size and overlap
   - Smaller chunks = more precise but potentially less context
   - Larger chunks = more context but potentially less precise

4. **Create Multiple Workspaces:**
   - Organize documents by topic or project
   - Prevents irrelevant documents from affecting answers

### Advanced Features to Explore

- **Custom prompts** to change how the AI responds
- **Multiple document types** (websites, YouTube transcripts, etc.)
- **Agent mode** for more autonomous task completion
- **API access** for integration with other tools

---

## Important Notes

- **Privacy:** Everything runs locally on your machine—no data is sent to external servers
- **Resource Usage:** Monitor your system resources (CPU/GPU/RAM) during use
- **Model Selection:** Smaller models are faster but less capable; find the right balance for your needs
- **Document Refinement:** This is a starting point—expect to refine your document collection and settings over time
- **Backup:** Consider backing up your AnythingLLM workspace folder periodically

---

## Additional Resources

- **AnythingLLM Documentation:** [docs.anythingllm.com](https://docs.anythingllm.com)
- **LMStudio Documentation:** [lmstudio.ai/docs](https://lmstudio.ai/docs)
- **Model Recommendations:** [lmstudio.ai/models](https://lmstudio.ai/models)
- **Community Support:** Join the AnythingLLM and LMStudio Discord communities for help and tips

---

**Congratulations!** You now have a working local RAG system. Start experimenting with different documents and questions to see what works best for your use case.
