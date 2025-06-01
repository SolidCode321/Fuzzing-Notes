This is the notes made in the planning phase for fuzzing.
## 1. [[Phase 1]]	

> Goal: Set up all tools needed to begin fuzzing ClamAV with file formats_

- [x] Define project scope: Fuzz ClamAV using file-format based fuzzing
- [x] Choose tools: ClamAV, Python, FuzzingBook
- [x] Create and activate a virtual environment
- [x] Install required Python packages (fuzzingbook, notebook, etc.)
- [x] Ensure Jupyter Notebook works inside the virtual environment
- [x] Install ClamAV system-wide (via Homebrew or package manager)
- [x] Fix freshclam.conf and update virus definitions
- [x] Fix ClamAV database permission issues
- [x] Verify clamscan works via terminal
- [x] Test clamscan with EICAR test file
## 2.[[Phase 2]]

>   Goal: Build and run a simple file-format fuzzer that integrates with ClamAV

 - [x] Create a simple Python fuzzer to generate random strings or byte sequences
 - [ ] Write fuzzed input to temporary files (e.g., .txt, .html, .pdf)
 - [x] Use subprocess to scan each file using clamscan
 - [x] Log input file, clamscan result (infected or not), and exit code
 - [x] Track and store samples that ClamAV flags or crashes on (if any)
 - [ ] Build a results folder or CSV log for fuzzer output and AV response
 - [ ] Add crash or error detection (parse ClamAV stdout/stderr)
 - [ ] Set time/memory limits for ClamAV scans (prevent hangs)
 - [ ] (Optional) Visualize detection rate with matplotlib / pandas
 - [ ] Read or review the “Random Fuzzer” chapter from FuzzingBook

## 3. [[Phase 3]]
