QUIZ OFFLINE - Android Studio source project (prototype v1.3)

USER CONFIGURATION
- Each imported file is stored as its own separate quiz set.
- The session covers every valid question in the selected file. There is no 50/100/500-question session cap.
- The review interval controls how often missed questions are repeated: 5, 10, 15, 20, or a custom interval (1-5000 new questions).
- A wrong answer shows the correct answer and automatically advances after about 1.1 seconds. The learner can advance immediately.
- Missed questions repeat until answered correctly, then the app continues through the remaining questions.
- Imported quiz sets are stored locally in IndexedDB and remain available after the app is closed, subject to Android app data not being cleared.
- The last selected quiz set is remembered.

SUPPORTED INPUT
- DOCX: reads explicit Word font colors in word/document.xml. Red values FF0000, C00000, and RED are accepted.
- PDF: reads text-based PDFs, including ToUnicode CMaps and split text fragments on a shared text baseline. It detects RGB red text. Some unusual PDFs, image-only scans, encrypted files, or complex layouts may still fail.
- Every question must begin with “Câu 1.” or “Question 1.” (number may vary) and contain exactly four answer paragraphs A-D, with exactly one red option.
- Wrapped text is supported when it remains on the same paragraph in DOCX, or on the same visual line/baseline in PDF. PDF text split into separate glyph runs is reassembled by baseline.

LARGE DOCUMENTS
- The app does not impose a 500-question ceiling. It parses the complete file and stores the recognized questions in a separate local quiz set.
- 500, 1,000, and several thousand questions are intended use cases. Actual performance depends on the phone, document size, and PDF complexity. Always compare the imported count with the source document before studying.
- If the import message reports skipped question headings, inspect those questions in the source file; malformed questions are not silently treated as valid.

SAMPLE VALIDATION
The supplied 5 cau.docx was inspected at the OOXML level: exactly one FF0000 answer option was present per question (B, D, B, C, C).
The supplied 5_cau.pdf was tested against the bundled PDF parser after adding ToUnicode CMap support and text-baseline reconstruction: 5 questions were parsed, with correct option indexes B, D, B, C, C.
These are sample-file checks, not a guarantee for every Word/PDF exporter.

CURRENT LIMITATIONS
- This is source code, not a compiled APK.
- It has not been compiled or tested on a physical Android device in this environment because Android SDK / Gradle build tools are unavailable here.
- The first build needs internet to download Android Gradle Plugin and Android SDK components. After building and installing, document parsing and quiz logic run locally without internet.
- A GitHub Actions workflow is included at .github/workflows/android-apk.yml. Upload/push this project to a GitHub repository, open Actions, run “Build Android APK”, then download the “QuizOffline-debug-apk” artifact. This avoids installing Android Studio on your own computer, but requires a GitHub account and internet access.
- Imported question banks persist locally, but active session position and score are not yet restored after force-closing the app.
- No OCR is included for scanned/image-only PDF files.

BUILD APK
OPTION A - AUTOMATIC BUILD WITH GITHUB ACTIONS
1. Create a GitHub repository and upload the contents of this project.
2. Open the repository Actions tab.
3. Run “Build Android APK” (or push to main/master).
4. When the workflow succeeds, open its run and download the “QuizOffline-debug-apk” artifact.
5. Extract the artifact ZIP and install app-debug.apk on the Android phone.

OPTION B - BUILD WITH ANDROID STUDIO
1. Extract this ZIP.
2. Open the “QuizOffline” folder in Android Studio.
3. Let Gradle sync and install Android SDK Platform 35 if Android Studio asks.
4. Select Build > Build Bundle(s) / APK(s) > Build APK(s).
5. Android Studio creates app-debug.apk under app/build/outputs/apk/debug/.
6. Transfer that APK to the Android phone and install it. Android may ask you to allow installation from that source.
