## Purpose

This Praat script attempts to detect potential vowel mispronunciations by comparing key acoustic features (formant frequencies F1 and F2) between a user's recording and a reference recording at specific points in time. Significant differences in F1 and F2 often correspond to differences in vowel articulation. *Note: This script uses the specific logic from the user-provided version.*

## How it Works

1.  **Input Selection:**
    *   Prompts the user to select the **Reference** audio file.
    *   Prompts the user to select the **User** audio file.
    *   Reads both files into Praat `Sound` objects.

2.  **Formant Object Creation:**
    *   Gets the total duration for both reference (`ref_duration`) and user (`user_duration`) sounds.
    *   Creates `Formant` objects for both the reference and user sounds using the `To Formant (burg)` command with specific parameters (max 5 formants, 5500 Hz ceiling, 0.025s window, 50 Hz pre-emphasis). These objects contain tracked formant frequencies over time.

3.  **Formant Value Extraction:**
    *   Calculates three specific time points relative to the total duration for each file: 25%, 50%, and 75%.
    *   For *both* the reference and user `Formant` objects, extracts the values of the first formant (F1) and second formant (F2) at each of these three calculated time points using `Get value at time`.

4.  **Difference Calculation:**
    *   Initializes accumulators for total formant differences (`f1_diff_total`, `f2_diff_total`) and counters for valid comparison points (`f1_points`, `f2_points`) to zero.
    *   Iterates through the three time points (25%, 50%, 75%):
        *   At each point, it checks if *both* the reference and user F1 values are defined (not `undefined`) and if the reference F1 is greater than 0 (to avoid division by zero). If valid, it calculates the absolute difference relative to the reference (`abs(ref_f1 - user_f1) / ref_f1`), adds this *ratio* to `f1_diff_total`, and increments `f1_points`.
        *   It performs the same check and calculation independently for F2 values, adding the ratio to `f2_diff_total` and incrementing `f2_points`.

5.  **Averaging and Thresholding:**
    *   Calculates the average F1 difference *percentage* (`avg_f1_diff_percent`) by dividing `f1_diff_total` by `f1_points` and multiplying by 100 (if `f1_points > 0`, otherwise result is `undefined`).
    *   Calculates the average F2 difference *percentage* (`avg_f2_diff_percent`) similarly.
    *   Calculates the overall average formant difference (`avg_formant_diff_percent`) by averaging `avg_f1_diff_percent` and `avg_f2_diff_percent` (only if both are defined).
    *   Sets a `mispronunciation_threshold_percent` (currently 25%).
    *   Sets a flag `mispronunciation_likely` to 1 (true) if `avg_formant_diff_percent` was successfully calculated and its value is greater than `mispronunciation_threshold_percent`. Otherwise, the flag remains 0 (false).

6.  **Output:**
    *   Clears the Praat Info window.
    *   Displays the file names.
    *   If the average formant differences could be calculated:
        *   Shows the average F1%, average F2%, and overall average formant difference %.
        *   Shows how many F1 and F2 points (out of 3) were used for the comparison.
        *   Prints a "Detection Result" indicating "LIKELY Mispronunciations Detected" or "Mispronunciations Not Detected".
        *   If likely, provides the reason: the calculated average difference exceeded the threshold, showing both values.
    *   If differences could *not* be calculated (due to insufficient valid points):
        *   States that the calculation failed and why.
        *   Prints "Detection Result: Undetermined".
    *   Includes notes about the simplified nature of this analysis.

7.  **Cleanup:**
    *   Removes the temporary `Sound` and `Formant` objects from the Praat object list.
