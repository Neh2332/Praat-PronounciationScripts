## Purpose

This Praat script aims to automatically detect potentially abnormal pausing patterns in a user's speech compared to a reference recording. It identifies abnormalities based on significant differences in either the *number* of pauses detected or the *average duration* of those pauses.

## How it Works

1.  **Input Selection:**
    *   Prompts the user to select the **Reference** audio file.
    *   Prompts the user to select the **User** audio file.
    *   Reads both files into Praat `Sound` objects.

2.  **Reference Pause Analysis:**
    *   Creates a `TextGrid` object from the reference sound using `To TextGrid (silences)` (100 Hz floor, -25 dB threshold, 0.1s min silent, 0.05s min sounding) to identify silent intervals.
    *   Iterates through all intervals in the reference `TextGrid`.
    *   Counts the number of intervals labeled "silent" (`ref_num_pauses`).
    *   Sums the duration of all "silent" intervals (`ref_pause_durations`).
    *   Calculates the average pause duration (`ref_avg_pause = ref_pause_durations / ref_num_pauses`), handling the case where `ref_num_pauses` is zero (setting `ref_avg_pause` to 0).

3.  **User Pause Analysis:**
    *   Performs the identical analysis steps on the user's `Sound` object.
    *   Creates a `user_textgrid`.
    *   Counts `user_num_pauses` and sums `user_pause_durations`.
    *   Calculates `user_avg_pause`.

4.  **Comparison and Detection Logic:**
    *   Calculates the absolute difference in the number of pauses (`pause_count_diff = abs(user_num_pauses - ref_num_pauses)`).
    *   Calculates a `pause_duration_ratio` comparing the average pause durations (`user_avg_pause / ref_avg_pause`), with specific handling if either average is zero.
    *   Calculates a `pause_pattern_score` (0-100) based on the `pause_duration_ratio`. The score is designed so that 100 represents identical average pause durations, and the score decreases as the ratio deviates from 1.0.
    *   Checks for the **abnormal break condition**: An abnormal break pattern is considered likely (`abnormal_breaks_likely` is set to true/non-zero) if **either** of the following is true:
        *   The absolute difference in the number of pauses is greater than 1 (`pause_count_diff > 1`).
        *   The `pause_pattern_score` (reflecting average duration similarity) is below 70 (`pause_pattern_score < 70`).

5.  **Output:**
    *   Clears the Praat Info window.
    *   Displays the file names.
    *   Shows the calculated pause counts, the difference in counts, the average pause durations (in milliseconds), and the calculated Pause Pattern Score.
    *   Prints a "DETECTION ASSESSMENT" indicating if abnormal breaks are "LIKELY Detected" or "NOT Detected".
    *   Provides reasoning explaining *which* condition (significant count difference OR low pattern score) triggered the detection, showing the relevant values.
    *   Includes notes about the reliance on silence detection parameters.

6.  **Cleanup:**
    *   Removes the temporary `Sound` and `TextGrid` objects from the Praat object list.
