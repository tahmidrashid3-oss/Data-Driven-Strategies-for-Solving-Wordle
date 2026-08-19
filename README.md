# Wordle Solver Comparison Project

## Reproducibility Materials

These reproducibility materials contain the computational resources used to reproduce the experiments reported in the MSc Data Science dissertation:

**Data-Driven Strategies for Solving Wordle**

The main analysis is contained in the Jupyter notebook:

`02_wordle_project.ipynb`

The notebook implements and evaluates five Wordle-solving strategies under a common simulation framework:

1. **First-candidate**
2. **Random**
3. **Frequency-based**
4. **Positional frequency**
5. **Approximate entropy**

The notebook reproduces the main solver results, runtime analyses, statistical comparisons, hard-case analyses, supplementary experiments, tables and figures reported in the dissertation.

---

## 1. Required Input Files

Place the following two word-list files in the same directory as the notebook:

- `wordle-answers-alphabetical.txt`
- `nyt-wordle-allowed-guesses-2026-03-06.txt`

The notebook expects:

- **2,315** possible Wordle solution words
- **14,855** allowable guess words

The data-loading section validates that all words are five-letter alphabetic strings, that there are no duplicate entries, and that every solution is also contained in the allowable-guess list.

---

## 2. Software Environment

The notebook was developed and run using:

- Python 3.13.1
- NumPy 2.2.2
- pandas 2.2.3

The notebook also uses:

- Matplotlib
- SciPy
- Pillow
- imageio

The core analysis requires NumPy, pandas, Matplotlib and SciPy. Pillow and imageio are used by the presentation-animation cells near the end of the notebook.

A suitable environment can be created with:

```bash
pip install numpy pandas matplotlib scipy pillow imageio jupyter
```

Exact runtime measurements may differ across machines and between executions because they depend on processor speed, operating-system scheduling, background processes and hardware state.

---

## 3. Reproducibility Settings

The main experiments use fixed random seeds:

```python
random.seed(42)
np.random.seed(42)
```

These settings make stochastic solver behaviour reproducible.

Seeds are reset where required for experiments involving stochastic behaviour. The random-seed sensitivity experiment separately evaluates the random solver across ten seeds.

Runtime measurements are not expected to be numerically identical between executions or across different hardware. Solver outcomes and other performance measures should otherwise remain reproducible when the same data, code and random seeds are used.

---

## 4. Running the Analysis

1. Install the required Python packages.
2. Place the notebook and both Wordle word-list files in the same directory.
3. Open the notebook in Jupyter Notebook, JupyterLab, VS Code or another compatible environment.
4. Restart the kernel.
5. Run the notebook from top to bottom in cell order.

For example:

```bash
jupyter notebook
```

Then open:

`02_wordle_project.ipynb`

and select **Run All**.

The notebook is sequential: later sections use functions and experiment objects created in earlier sections, so cells should not normally be run out of order.

A complete run may take a substantial amount of time because several sections repeat the full set of solver experiments.

---

## 5. Notebook Structure

### 1. Data Loading and Validation

Loads and validates the solution and allowable-guess dictionaries.

### 2. Wordle Feedback Function

Implements Wordle's green, yellow and grey feedback rules, including correct handling of repeated letters.

### 3. Candidate Filtering

Retains only solution words consistent with all feedback observed so far.

### 4. Game Simulation Framework

Runs complete Wordle games using a common game engine and six-guess stopping rule.

### 5. Baseline Solver Strategies

Implements the first-candidate and random baselines.

### 6. Experimental Evaluation Function

Evaluates each strategy and records success rate, average guesses, worst case, unsolved games, runtime and game-level results.

### 7. Frequency-Based Solver

Ranks remaining candidates using overall letter frequencies.

### 8. Positional Frequency Solver

Ranks remaining candidates using position-specific letter frequencies.

### 9. Approximate Entropy Solver

Uses the fixed opening word `SLATE` and then entropy-based selection over the remaining candidate set.

### 10. Entropy Feasibility and Runtime Analysis

Includes the exhaustive-versus-approximate entropy feasibility experiment, entropy walkthrough, repeated runtime benchmarking, candidate-set profiling and performance-runtime analysis.

### 11-14. Overall Results and Statistical Analysis

Produces the overall solver comparison, failure and hard-case analyses, Wilcoxon signed-rank tests and final summary tables.

### 15. Report Figures and Saved Outputs

Generates the principal figures and CSV outputs used to verify the dissertation results.

### 16. Reproducibility Information

Prints information about the Python environment used to run the analysis.

### 17. Additional Experiments

Contains:

- Sensitivity of the approximate entropy solver to the opening word
- Random-seed sensitivity
- Removal of the six-guess limit
- Full Hard Mode evaluation across all 2,315 target words

The final cells create presentation GIFs. These are not required to reproduce the core research findings and may be skipped if only the dissertation analysis is being reproduced.

---

## 6. Main Generated Outputs

The notebook creates figures and CSV files for checking the reported analysis.

### Main Figures

- `wordle_example_game.png`
- `candidate_filtering_example.png`
- `simulation_flowchart.png`
- `entropy_partition_comparison.png`
- `baseline_solver_comparison.png`
- `solver_performance_boxplot.png`
- `performance_runtime_tradeoff.png`
- `candidate_reduction_frequency_positional.png`
- `decision_time_by_candidate_size.png`
- `comparison_with_bertsimas.png`

### Main Result Tables and CSV Outputs

Outputs include results relating to:

- Overall solver performance
- Wilcoxon signed-rank tests
- Entropy feasibility
- Failure overlap and hard cases
- Runtime profiling
- Candidate-set reduction

### Supplementary Outputs

Outputs include results relating to:

- Opening-word sensitivity
- Random-seed sensitivity
- Removal of the six-guess limit
- Hard Mode evaluation

Figures and outputs are written to the relevant `figures/` and `outputs/` directories. Required output folders are created automatically where needed.

Generated figures and CSV files do not need to be present before the notebook is run.

---

## 7. Expected Experimental Setup

Unless explicitly stated otherwise in a supplementary experiment, the main evaluation uses:

- **2,315** target solution words
- **14,855** allowable guesses
- Normal Wordle mode
- A maximum of six guesses
- Identical feedback and candidate-filtering logic for every solver
- Fixed random seed 42
- Average guesses calculated over solved games only

The first-candidate, random, frequency-based and positional frequency solvers select guesses from the remaining candidate solutions.

The approximate entropy solver uses `SLATE` as a fixed opening guess and then selects subsequent guesses from the remaining candidate set using expected information gain.

Only the guess-selection strategy changes between the five principal solvers.

---

## 8. Runtime Notes

Some sections are computationally expensive.

In particular:

- Every main solver is evaluated across all 2,315 solution words.
- The runtime benchmarking section repeats solver evaluations.
- Entropy calculations are considerably more expensive than the frequency-based methods.
- The exhaustive entropy feasibility experiment is intentionally limited to five targets because exhaustive evaluation is computationally costly.
- Supplementary experiments may repeat solver evaluations across multiple settings.
- The Hard Mode evaluation compares all five solvers across all 2,315 target words in both normal and Hard Mode.

A complete **Run All** may therefore take a substantial amount of time, depending on the computer being used.

Runtime measurements are inherently sensitive to processor speed, operating-system scheduling, background activity and hardware state. Consequently, exact wall-clock runtimes are not expected to be identical between executions or across different computers.

The dissertation reports runtime measurements obtained from the benchmarking executions used for the submitted analysis. Differences in runtime alone do not indicate a failure of reproducibility if the substantive solver outcomes remain consistent.

---

## 9. Checking Successful Reproduction

A successful reproduction should:

1. Load 2,315 solution words and 14,855 allowable guesses.
2. Pass the feedback and candidate-filtering validation checks.
3. Complete the five main solver experiments.
4. Generate the overall solver comparison and statistical tests.
5. Reproduce the reported figures and analysis outputs.
6. Complete the supplementary experiments where those cells are run.

The principal solver-performance results should reproduce:

| Solver | Success Rate | Average Guesses | Unsolved |
|---|---:|---:|---:|
| First-candidate | 97.5% | 4.275 | 58 |
| Random | 98.3% | 4.029 | 40 |
| Frequency-based | 99.0% | 3.650 | 22 |
| Positional frequency | 99.4% | 3.619 | 14 |
| Approximate entropy | **99.7%** | **3.518** | **7** |

Exact wall-clock runtimes may vary between executions, but the substantive solver-performance results should remain consistent when the same input data, code and random seeds are used.

---

## 10. Supplementary Experiments

### Opening-Word Sensitivity

The approximate entropy solver is evaluated using six fixed opening words:

- `SLATE`
- `TRACE`
- `CRANE`
- `STARE`
- `RAISE`
- `ARISE`

This experiment examines whether the main entropy results depend strongly on the choice of opening word.

### Random-Seed Sensitivity

The random solver is evaluated using ten different random seeds to measure variation caused by stochastic guess selection.

### Six-Guess-Limit Experiment

The maximum number of guesses is increased from six to twenty. All five solvers eventually identify all 2,315 targets, allowing the main unsolved games to be interpreted in relation to Wordle's six-guess constraint.

### Hard Mode Evaluation

All five solvers are evaluated in both normal and Hard Mode across the complete set of 2,315 target words.

Because the main solvers select subsequent guesses from the remaining candidate set, their guess sequences are already consistent with previous feedback. The experiment therefore produces identical guess sequences under the Hard Mode constraints implemented in the notebook.

---

## 11. Data Provenance

The Wordle dictionaries are based on publicly available Wordle word lists compiled by cfreshman and used as fixed input data for the study.

The dissertation contains the full citation and access information for these data sources.

The input files remain fixed throughout the experiments so that every solver is evaluated using the same solution and allowable-guess dictionaries.

---

## 12. Notes for Reproducibility

The notebook should be treated as an executable research record rather than as a collection of independent code snippets. Running it sequentially from a fresh kernel is the recommended way to reproduce the analysis.

If an error reports that one of the two word-list files cannot be found, check that both `.txt` files are in the notebook's current working directory.

Because runtime measurements are sensitive to the execution environment, rerunning the notebook may produce different runtime values even when all solver outcomes are unchanged. This is expected.

The dissertation reports runtime measurements from the benchmarking executions used for the submitted analysis. Exact wall-clock values are not expected to reproduce across different executions or hardware; reproducibility is instead established through the same experimental methodology, solver behaviour, performance results, analyses and conclusions.
