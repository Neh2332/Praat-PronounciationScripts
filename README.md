# Praat Basic Pronunciation Assessment Scripts

## Overview

This repository contains a set of four scripts designed to be run in the **Praat** software application. These scripts provide basic, automated analyses to compare a user's speech recording against a reference recording (presumably representing correct pronunciation) for the same utterance. They aim to detect potential issues such as word omissions, word insertions, abnormal pausing patterns, and significant vowel mispronunciations based purely on acoustic analysis.

## What is Praat?

[Praat](https://www.fon.hum.uva.nl/praat/) (Dutch for "talk") is a free, cross-platform scientific software package extensively used by linguists, phoneticians, and speech scientists for the analysis, synthesis, and manipulation of speech and other sounds. It offers powerful visualization tools (like spectrograms and waveforms) and a wide array of functions for acoustic measurement (pitch, formants, intensity, duration, etc.). Praat also includes a scripting language that allows users to automate complex analysis tasks, which is what these scripts utilize.

## How These Scripts Can Be Useful

These scripts offer a first pass, automated way to flag potential areas of concern in a learner's pronunciation when compared to a model pronunciation. They can be useful for:

*   **Language Learners:** Getting quick, objective feedback on specific aspects like timing, fluency breaks, and vowel articulation compared to a native or target speaker.
*   **Teachers/Tutors:** Quickly screening recordings for common issues before delving into more detailed manual analysis or providing targeted feedback.
*   **Speech Research (Basic):** Performing batch comparisons of simple acoustic features related to fluency and articulation across different speakers or conditions.

**Important:** These scripts provide *acoustic estimations* of potential issues, not definitive linguistic judgments. They should be used as a supplementary tool alongside human listening and evaluation.

## Scripts Included

This set includes four distinct scripts, each focusing on a specific aspect:

1.  **`Word_Omission_Detection.praat`**
    *   **Purpose:** Detects if the user recording is likely missing words compared to the reference.
    *   **Method:** Compares the number of detected "sounding" segments (approximating word/syllable chunks) and the overall duration ratio between the user and reference files. An omission is flagged if the user has fewer segments *and* their recording is significantly shorter (duration ratio < 0.9).

2.  **`Word_Insertion_Detection.praat`**
    *   **Purpose:** Detects if the user recording likely contains extra words or sounds compared to the reference.
    *   **Method:** Compares the number of "sounding" segments and the overall duration ratio. An insertion is flagged if the user has more segments *and* their recording is significantly longer (duration ratio > 1.1).

3.  **`Abnormal_Breaks_Detection.praat`**
    *   **Purpose:** Detects unusual pausing patterns (silences) in the user recording compared to the reference.
    *   **Method:** Compares both the *number* of detected pauses and the *average duration* of those pauses. An abnormal pattern is flagged if the difference in the *number* of pauses is greater than 1, *or* if the *average pause duration* is significantly different (indicated by a "Pause Pattern Score" based on the ratio of average durations falling below 70).

4.  **`Mispronunciation_Detection_Formant.praat`**
    *   **Purpose:** Detects potential vowel mispronunciations by comparing key acoustic correlates of vowel quality.
    *   **Method:** Measures the first two formant frequencies (F1 and F2) – which strongly relate to vowel articulation – at three relative time points (25%, 50%, 75%) in both recordings. It calculates the average percentage difference for F1 and F2 across comparable points. A mispronunciation is flagged if the *overall average percentage difference* (average of F1 and F2 average differences) exceeds a threshold (currently set at 25%).

## How to Use the Scripts

1.  **Install Praat:** Download and install Praat from the official website: [https://www.praat.org/](https://www.fon.hum.uva.nl/praat/)
2.  **Prepare Audio Files:** You need two audio files for each comparison:
    *   A **Reference** file: The target pronunciation (e.g., recorded by a native speaker).
    *   A **User** file: The pronunciation you want to evaluate.
    *   Ensure both files contain recordings of the *same* word, phrase, or sentence.
    *   Standard audio formats like WAV or AIFF are recommended for best compatibility.
3.  **Open Praat:** Launch the Praat application. You will typically see two windows: "Praat Objects" and "Praat Picture".
4.  **Run a Script:**
    *   Go to the `Praat` menu in the "Praat Objects" window.
    *   Select `Open Praat script...`.
    *   Navigate to and select the desired script file (e.g., `Word_Omission_Detection.praat`). A script editor window will open.
    *   In the script editor window, go to the `Run` menu and select `Run` (or press `Ctrl+R` / `Cmd+R`).
5.  **Select Files:** The script will pause and prompt you twice:
    *   First, to select the **REFERENCE** audio file.
    *   Second, to select the **USER** audio file.
    *   Click `OK` after each selection dialog appears, then choose the appropriate file.
6.  **View Results:** The script will perform the analysis and display the results in the **Praat Info** window (if it's not visible, go to `Window` -> `Show info window` in the "Praat Objects" window).

## Understanding the Output

Each script will output results to the Praat Info window, typically including:

*   The names of the reference and user files being compared.
*   Key acoustic metrics calculated (e.g., duration, segment counts, pause counts, average pause duration, formant differences).
*   A **Detection Assessment**: This will state whether the specific issue (omission, insertion, abnormal break, mispronunciation) is "LIKELY Detected", "NOT Detected", or potentially "Undetermined" (if calculations couldn't be performed).
*   **Reasoning:** Crucially, the output explains *why* a detection was flagged or not, by referring to the calculated metrics and comparing them against the thresholds defined within the script (e.g., duration ratio < 0.9, segment count difference, pause score < 70, average formant difference > 25%).
*   **Explanatory Notes:** The output includes notes reminding the user about the nature of the analysis (acoustic estimation) and its dependency on recording quality and analysis parameters.

Examples of the scripts in action comparing two AI Generated Voices of EricGood1.wav and LiamGood2.wav

**`Word_Omission_Detection.praat`**

![image](https://github.com/user-attachments/assets/c65d40b4-b17c-4ade-8eda-b8832b508c5f)

**`Word_Insertion_Detection.praat`**

![image](https://github.com/user-attachments/assets/16aca52b-e26a-4d87-865c-9b01880ec325)

**`Abnormal_Breaks_Detection.praat`**

![image](https://github.com/user-attachments/assets/5c376859-d731-4ba4-9a86-0d37f2e128fe)

**`Mispronunciation_Detection_Formant.praat`**

![image](https://github.com/user-attachments/assets/d231b4c5-0848-488e-9d93-b5513050ef73)


## Limitations and Disclaimer

*   **Acoustic Analysis Only:** These scripts operate purely on the acoustic signal. They do not understand language, grammar, or context. A detected "segment" might not perfectly correspond to a word or syllable.
*   **Sensitivity to Recording Quality:** Background noise, low volume, or microphone variations can significantly impact the accuracy of silence detection, segment counting, and formant analysis. Use clear, comparable recordings where possible.
*   **Parameter Dependency:** The accuracy of segmentation (`To TextGrid (silences)`) and formant analysis (`To Formant (burg)`) depends on parameters set within the scripts (e.g., silence threshold, formant ceilings). These default values may not be optimal for all speakers, languages, or recording conditions.
*   **Formant Analysis Simplification:** The mispronunciation script only analyzes F1/F2 at three specific time points. It doesn't analyze consonant sounds, dynamic formant movements, or other acoustic cues relevant to pronunciation. Formant tracking itself can be error-prone.
*   **Fixed Thresholds:** The thresholds used for detection (e.g., 0.9/1.1 duration ratio, pause count difference > 1, pause score < 70, formant difference > 25%) are heuristic values. They might be too strict or too lenient depending on the specific application and may require adjustment for optimal performance.
*   **Not a Comprehensive Assessment:** These scripts provide isolated checks. They do not offer an overall pronunciation score or analyze aspects like intonation, stress, or rhythm beyond the specific metrics calculated (e.g., average pause duration).


## Requirements

*   **Praat software:** Version 6.0 or later recommended ([https://www.praat.org/](https://www.fon.hum.uva.nl/praat/)).
*   **Audio Files:** A reference audio file and a user audio file for comparison, ideally in WAV or AIFF format.

## License

These scripts are provided under the MIT License. See the LICENSE file (if included) or refer to the standard MIT License terms. You are free to use, modify, and distribute them, but they come with no warranty.
