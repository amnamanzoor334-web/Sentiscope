SentiScope

Understand the sentiment behind customer feedback.

SentiScope is an educational AI web application that classifies English customer reviews as Positive, Negative, or Neutral. It combines a trained machine-learning model with a responsive dark dashboard, lime accents, interactive analysis, and feedback filtering. Predictions run directly in your browser.

SentiScope dashboard

Run it immediately

1. Extract the ZIP.
2. Open the SentiScope_GitHub folder.
3. Double-click index.html in a modern browser.
4. Open Review analyzer, type an English review, and select Analyze sentiment.
5. Select Add to feedback collection to update the dashboard.

No API key, paid account, database, npm installation, or Python installation is required to use the included app. The pretrained model is already bundled. Keep all app files in the same folder. JavaScript must be enabled.

If your browser restricts local scripts, serve the folder with Python:

python -m http.server 8000

Then open http://localhost:8000. On Windows, py -m http.server 8000 also works.

Features

• Positive, negative, and neutral classification for individual English reviews.
• A breakdown of the model scores across all three classes.
• Twelve synthetic sample reviews with predicted categories.
• Session totals, category counts, and a distribution chart.
• Category filters and resetting the sample collection.
• Adding your analyzed review to the current collection.
• Input validation and safe rendering of entered text.
• Responsive layouts for desktop and mobile screens.

The app starts with sample data. Reviews stay in browser memory and reset on refresh; they are not sent to a server. CSV upload and a database are not included.

Technology stack

|Component            |Technology                               |
|---------------------|-----------------------------------------|
|Interface            |HTML5, CSS3, vanilla JavaScript          |
|Training             |Python and scikit-learn                  |
|Text features        |TF-IDF with word unigrams and bigrams    |
|Classifier           |Logistic regression                      |
|Browser inference    |JavaScript with exported model parameters|
|Optional local server|Node.js 18+ or Python                    |

How the model works

Python trains a scikit-learn TF-IDF vectorizer using word unigrams and bigrams and a logistic regression classifier. Vocabulary, IDF weights, coefficients, and intercepts are exported to model.js. sentiment.js runs the same feature transformation, normalization, and classifier in the browser. This is a trained model; the frontend does not assign sentiment using a fixed keyword list.

Training uses 180 synthetic sentences derived from 60 author-written base examples. The training data is included in training_data.json. No real customer data is included. This small demonstration model has not been evaluated on an independent benchmark. Model scores are not measured accuracy or calibrated confidence. Sarcasm, mixed sentiment, unfamiliar terms, and Roman Urdu may be misclassified. Human review is needed before using predictions for business decisions.

Run with Node.js

If you have Node.js 18 or newer, run these commands from the project folder:

npm start

Open http://localhost:8000. No npm dependencies need to be installed. To choose a different port:

npm start -- --port 3000

Using SentiScope

1. View the sample feedback and sentiment distribution in Overview.
2. Open Review analyzer and enter an English review (up to 2,000 characters).
3. Select Analyze sentiment to see the predicted category and three class scores.
4. Select Add to feedback collection to update the dashboard.
5. Filter reviews by category or reset the collection to the original samples.

Screenshots

Positive review

Positive sentiment result

Negative review

Negative sentiment result

Neutral review

Neutral sentiment result

Retrain the model

Python 3.11 or newer is required only for training and the model parity check.

python -m venv .venv

Activate the environment:

# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS or Linux
source .venv/bin/activate

Then:

python -m pip install -r requirements.txt
python train_model.py

Reload the app after training. The script contains the base example corpus and writes training_data.json and model.js next to itself. To change the training examples, edit the base dictionary in train_model.py. Editing training_data.json alone does not change the training script’s corpus.

Verification

With Node.js and Python dependencies installed:

node tests/test_model.cjs
python tests/check_parity.py

These checks cover input validation, sentiment examples, output probabilities, browser-versus-Python inference parity, and required app files. They are implementation checks, not proof of model accuracy on real-world data.

Repository setup

Upload the contents inside SentiScope_GitHub to your repository root, including the screenshots/ and tests/ folders. Place this file at the root as README.md. Keep the relative folder structure so the app assets and screenshot links work.

Suggested repository description:

> An educational AI customer feedback sentiment analysis app with browser inference and an interactive dashboard.

Files

|File                |Purpose                                               |
|--------------------|------------------------------------------------------|
|`index.html`        |App interface and asset loading                       |
|`style.css`         |Dashboard styling and responsive layouts              |
|`app.js`            |User interactions and session state                   |
|`sentiment.js`      |Browser and Node inference engine                     |
|`model.js`          |Bundled pretrained model parameters                   |
|`train_model.py`    |Reproducible Python model training                    |
|`training_data.json`|Synthetic labelled training sentences                 |
|`requirements.txt`  |Python training dependency                            |
|`package.json`      |npm run and test commands                             |
|`server.mjs`        |Optional local static-file server                     |
|`tests/`            |Model validation and Python parity checks             |
|`screenshots/`      |Dashboard, sentiment results, and workflow screenshots|

Internship use

This repository contains an educational demonstration. Describe the work and assistance you actually used, learn the workflow, and confirm the project with your internship supervisor before reporting it as internship work. The screenshots illustrate the interface and example predictions, not completed company activities or benchmark results.

Possible future improvements

• Train and evaluate on a larger, independently labelled review dataset.
• Add CSV import and export.
• Add persistent feedback storage.
• Explore multilingual sentiment analysis and model explanations.

These are proposed extensions and are not included in the current version.
