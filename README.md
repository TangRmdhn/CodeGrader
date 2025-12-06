# ⚡ Automatic Code Grader Assistant

A web application to automatically grade programming assignments using **Groq** AI. This system can read questions from PDF or text, process a *batch* of code files (in `.zip`), and provide detailed *feedback* along with scores for each file.

<br>

## 🎯 Problems Solved

Grading programming assignments manually presents several challenges, especially when dealing with hundreds of students:

1.  **Time-Consuming**: Opening files one by one, running code, and checking logic takes hours.
2.  **Grading Inconsistency**: Human fatigue can lead to inconsistent grading between the first and the last student.
3.  **Limited Feedback**: Students often only receive a numerical score without knowing the details of their logical errors due to the grader's time constraints.

**CodeGrader** is here to solve these problems by automating the reading process, logic execution, and providing instant feedback.

<br>

## 🎥 Quick Demo

See how this app grades dozens of codes in just seconds:

![CodeGrader Demo](https://placeholder-url.com/replace-with-your-gif-link-here.gif)

*(Note: Replace the link above with your actual application demo GIF/Image URL)*

<br>

## 🏗️ System Architecture

Below is the workflow (pipeline) of how the system processes data from input to the grading report:

```mermaid
graph TD
    User([👨‍🏫 User / Lecturer])
    subgraph UI [Frontend - Streamlit]
        InputSoal[Input Question / Upload PDF]
        InputZip[Upload Student ZIP]
        ResultTable[Real-time Result Table]
    end

    subgraph Backend [Backend Processing]
        Extractor[📂 ZIP Extractor]
        PDFReader[📄 PDF Parser]
        PromptEng[⚙️ Prompt Engineering]
        JSONParser[📝 JSON Cleaner & Validator]
    end

    subgraph AI [External API]
        GroqAPI[⚡ Groq Inference Engine]
        LLM[(🤖 LLM: Llama/Mixtral)]
    end

    User --> InputSoal
    User --> InputZip
    InputSoal --> PDFReader
    InputZip --> Extractor
    
    PDFReader --> PromptEng
    Extractor -- Loop per File Code --> PromptEng
    
    PromptEng -- Request + Context --> GroqAPI
    GroqAPI <--> LLM
    GroqAPI -- Raw Response --> JSONParser
    
    JSONParser -- Validated Data --> ResultTable
    ResultTable -- Export XLSX/CSV --> User
````

<br>

## ✨ Key Features

  - 🤖 **Automatic AI Grading**: Utilizes super-fast LLM models from Groq for accurate and consistent grading.
  - 📄 **PDF & Text Support**: Read questions directly from `.pdf` files or copy-paste text.
  - 📦 **Batch Processing**: Grade dozens or hundreds of assignment files at once with just a single `.zip` file.
  - ⚡ **Real-time Progress**: View grading progress and results coming in one by one *live*.
  - 📊 **Statistics & Visualization**: Get statistical summaries (average, highest, lowest) and color-coded result tables.
  - 📤 **Export Results**: Download the full grading report in `.xlsx` (Excel) or `.csv` format.
  - 🔧 **Model Configuration**: Choose the Groq model that best suits your needs, from the fastest to the most accurate.

## 🚀 Installation & Setup

This is a complete guide to running the application on your local machine.

### Prerequisites

  - **Python 3.8** or newer.
  - **Groq API Key**: You can get it for free at the [Groq Console](https://console.groq.com/keys).

-----

### Step 1: Clone Repository

Open your terminal or Command Prompt, then *clone* this repository to your computer and enter the directory.

```bash
git clone [https://github.com/TangRmdhn/asisten-penilai-kode.git](https://github.com/TangRmdhn/asisten-penilai-kode.git)
cd asisten-penilai-kode
```

### Step 2: Create Virtual Environment (Highly Recommended)

Creating a *virtual environment* (venv) is a *best practice* to isolate your project dependencies.

```bash
# Create venv in a folder named 'venv'
python -m venv venv
```

Next, activate the venv:

  - **Windows (Command Prompt):**
    ```bash
    venv\Scripts\activate
    ```
  - **macOS / Linux (Bash):**
    ```bash
    source venv/bin/activate
    ```

You will see `(venv)` at the beginning of the terminal line if successful.

### Step 3: Install Dependencies

Make sure your venv is active, then install all required *libraries* from `requirements.txt`.

```bash
pip install -r requirements.txt
```

### Step 4: Configure API Key

The application reads the API Key from the `.env` file.

1.  Create a new file named `.env` in the project's root folder (same location as `app.py`).
2.  Open the `.env` file with a text editor and add the following line:

<!-- end list -->

```env
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Replace `gsk_xxxxxxxx...` with the Groq API Key you obtained from the [Groq Console](https://console.groq.com/keys).

## 💻 Running the Application

After all setup is complete, run the application using Streamlit:

```bash
streamlit run app.py
```

The application will automatically open in your browser (usually at `http://localhost:8501`).

## 📖 How to Use

1.  **Input Question**: On the left side, select the "Text Input" tab to type the question, or "Upload PDF" to upload the question file.
2.  **Additional Criteria (Optional)**: Enter important points that must be graded by the AI (e.g., "Must use recursion", "Variable names must be clear").
3.  **Upload Assignment File**: Compress all student code files (e.g., `Ahmad.py`, `Budi.py`) into **one .zip file** then upload the ZIP file.
4.  **Select Model (Optional)**: On the left sidebar, you can choose the AI model you want to use.
5.  **Start Grading**: Click the **"🚀 Start Grading"** button.
6.  **View Results**: Results will appear one by one in the table on the right in *real-time*. The table will be color-coded for easier analysis.
7.  **Download Report**: Once finished, grading statistics will appear. Use the "Download Excel" or "Download CSV" buttons to save the report.

**Recommended `.zip` Structure:**

```
student_assignments.zip
├── 2024001_Ahmad.py
├── 2024002_Budi.py
├── SubFolder/2024003_Citra.py  <-- (The app can read files inside sub-folders)
└── ...
```

## ⚙️ Advanced Configuration

### Model Options

You can select different models in the sidebar. Each model has its advantages:

| Model | Description |
| :--- | :--- |
| `openai/gpt-oss-120b` | ✅ **Default & Recommended**. Largest & best model for high accuracy. |
| `llama-3.3-70b-versatile` | ⚡ Fast model with great performance. |
| `llama-3.2-90b-text-preview` | 🔬 Experimental model with 90B parameters. |
| `llama-3.1-70b-versatile` | 💪 Stable model for various tasks. |
| `mixtral-8x7b-32768` | 🎯 MoE model with long context (suitable for very long code). |
| `gemma2-9b-it` | 💎 Lightweight model from Google, very fast. |
| `gemma-7b-it` | ⚡ Lightest & fastest model (suitable for very large *batches*). |

### Temperature

Currently, `temperature` is statically set to **`0.1`** inside `app.py`. This low value is chosen to ensure the AI provides consistent, objective grading and is not too "creative" between files.

## 🤝 Contributing

Contributions are very welcome\! If you want to develop new features or fix bugs:

1.  *Fork* this repository.
2.  Create a new *branch* (`git checkout -b feature/CoolFeature`).
3.  *Commit* your changes (`git commit -m 'Add CoolFeature'`).
4.  *Push* to the branch (`git push origin feature/CoolFeature`).
5.  Create a *Pull Request*.

## 👨‍💻 Author

**Bintang Ramadhan**

  - GitHub: [@TangRmdhn](https://github.com/TangRmdhn)
  - Email: bintangramadhan0710@gmail.com
  - LinkedIn: [Bintang Ramadhan](https://www.linkedin.com/in/tang-ramadhan)

## 🙏 Acknowledgments

  - **[Groq](https://groq.com/)** for the incredibly fast AI inference platform.
  - **[Streamlit](https://streamlit.io/)** for the simple and cool Python web framework.
  - **[Meta AI](https://ai.meta.com/llama/)** & **[Google](https://www.google.com/search?q=https://ai.google/gemma/)** for powerful *open-source* models.

-----

⭐ If this project helps you, don't hesitate to give it a *star* on GitHub\!

```
```
