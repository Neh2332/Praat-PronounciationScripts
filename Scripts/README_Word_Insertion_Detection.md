## Purpose

This Praat script aims to automatically detect potential instances where a user may have inserted extra words, sounds, or significant speech segments when compared to a reference audio recording of the same utterance. It bases its detection on analyzing and comparing the number of detected speech chunks and the overall duration of both recordings.

## How it Works

1.  **Input Selection:**
    *   The script prompts the user to select the **Reference** audio file (target pronunciation).
    *   It then prompts for the **User** audio file (pronunciation to evaluate).
    *   It reads both files into Praat `Sound` objects.

2.  **Reference Audio Analysis:**
    *   Calculates the total duration of the reference `Sound` (`ref_duration`).
    *   Segments the reference audio into "silent" and "sounding" intervals using `To TextGrid (silences)` with specific parameters (100 Hz floor, -25 dB threshold, 0.1s min silent, 0.05s min sounding).
    *   Counts the number of "sounding" intervals in the reference `TextGrid` (`ref_speech_segments`), approximating the number of speech chunks.

3.  **User Audio Analysis:**
    *   Performs the identical analysis on the user's `Sound` object.
    *   Calculates `user_duration`.
    *   Creates a `user_textgrid` using the same segmentation parameters.
    *   Counts the "sounding" intervals in the user's TextGrid (`user_speech_segments`).

4.  **Comparison and Detection Logic:**
    *   Calculates the `duration_ratio` (`user_duration / ref_duration`), handling potential zero reference duration. A ratio > 1 indicates the user recording is longer.
    *   Checks for the **insertion condition**: Insertion is considered likely (`word_insertions_likely` is set to true/non-zero) **if and only if** *both* of the following are true:
        *   The user has more speech segments than the reference (`user_speech_segments > ref_speech_segments`).
        *   The user's recording is significantly longer than the reference (`duration_ratio > 1.1`). The 1.1 threshold means the user's recording is more than 110% of the reference length.

5.  **Output:**
    *   Clears the Praat Info window.
    *   Displays the names of the files.
    *   Shows the calculated durations, duration ratio, and speech segment counts.
    *   Prints a "DETECTION ASSESSMENT" indicating if word insertions are "LIKELY Detected" or "NOT Detected".
    *   Provides reasoning based on whether the two conditions (more segments AND significantly longer duration) were met, showing the values.
    *   Includes notes about the estimation nature and parameter dependency.

6.  **Cleanup:**
    *   Removes the temporary `Sound` and `TextGrid` objects from the Praat object list.
