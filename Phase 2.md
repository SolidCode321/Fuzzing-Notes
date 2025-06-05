Created a basic random fuzzer:

run_fuzzer_pipeline.py:
```
# run_fuzzer_pipeline.py
from fuzzers.random_fuzzer import generate_random_string, save_to_file
from fuzzers.clamav_scanner import scan_with_clamav
import csv
import os
  
# Prepare logs directory
logfile = 'logs/scan_results.csv'
os.makedirs('logs', exist_ok=True)

# Set up CSV for logging results
with open(logfile, 'w', newline='') as csvfile:
fieldnames = ['file', 'infected', 'crashed', 'returncode', 'stdout', 'stderr']
writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
writer.writeheader()

# Fuzz and scan loop
for i in range(10): # Adjust number of iterations as needed
fuzz = generate_random_string(200)
file_path = save_to_file(fuzz) # Saves to ../samples/fuzzed_*.txt
result = scan_with_clamav(file_path)

infected = 'FOUND' in result.get('stdout', '')
crashed = result.get('returncode', 0) != 0

writer.writerow({
'file': file_path,
'infected': infected,
'crashed': crashed,
'returncode': result.get('returncode'),
'stdout': result.get('stdout'),
'stderr': result.get('stderr')
})

print(f"[{i+1}] Scanned: {file_path} | Infected: {infected} | Crashed: {crashed}")
```

random_fuzzer.py:
```
# fuzzers/random_fuzzer.py
import os
import random
import string

def generate_random_string(length=100):
return ''.join(random.choices(string.printable, k=length))

def save_to_file(content, directory=os.path.join(os.pardir, 'samples/fuzzed'), extension='.txt'):
os.makedirs(directory, exist_ok=True)
file_path = os.path.join(directory, f'fuzz_{random.randint(1000,9999)}{extension}')
with open(file_path, 'w') as f:
f.write(content)
return file_path

if __name__ == "__main__":
for _ in range(10): # Generate 10 samples
fuzz = generate_random_string()
path = save_to_file(fuzz)
print(f"Generated: {path}")
```

clamav_scanner.py:
```
# fuzzers/clamav_scanner.py
import subprocess

def scan_with_clamav(file_path):
result = subprocess.run(
['clamscan', '--no-summary', '--infected', file_path],
capture_output=True,
text=True
)
return {
'file': file_path,
'stdout': result.stdout.strip(),
'stderr': result.stderr.strip(),
'returncode': result.returncode
}

if __name__ == "__main__":
import sys
if len(sys.argv) < 2:
print("Usage: python clamav_scanner.py <file_path>")
else:
result = scan_with_clamav(sys.argv[1])
print(result)
```

