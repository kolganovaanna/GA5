# Graded Assignment #5

## General information

- Author: Anna kolganova
- Date: 2025-11-01
- Environment: Pitzer cluster at OSC via VS Code
- Working dir: `/fs/ess/PAS2880/users/kolganovaanna/GA5`

## Assignment background

This assignment targets using generative AI in coding

## Protocol

**PART A**

I used the following commands to set up (questions 1-4)

```bash
mkdir GA5/
cd GA5/
touch README.md
```

I used the following commands to initialize a git repository. I will make a results and data directories and add it to .gitignore. 

```bash
mkdir results data
git init
git add README.md
git commit -m "Committing to README to set up"
echo "results/" > .gitignore
echo "data/" >> .gitignore
git add .gitignore
git commit -m "Adding a Gitignore file"
```

4. Explore the use of genAI tools to help you with coding tasks and and document your trials.

*Trial 1*

Q1: # You are a Unix shell master who helps students write code in bash. Tell me how to find a directory for a specific file (let's say ERR10802885_R1.fastq.gz ) and print it

AI used: Github Copilot (agent)

What Copilot aswered: 

```bash
# print full path(s)
find /fs/ess/PAS2880/users/kolganovaanna -type f -name 'ERR10802885_R1.fastq.gz' 2>/dev/null

# print only the directory(ies) that contain the file
find /fs/ess/PAS2880/users/kolganovaanna -type f -name 'ERR10802885_R1.fastq.gz' -printf '%h\n' 2>/dev/null | sort -u

# full paths (may be slow)
sudo find / -type f -name 'ERR10802885_R1.fastq.gz' 2>/dev/null

# first match only (faster)
find / -type f -name 'ERR10802885_R1.fastq.gz' -print -quit 2>/dev/null

# case-insensitive name match
find /fs/ess/PAS2880/users/kolganovaanna -type f -iname 'err10802885_r1.fastq.gz' -print 2>/dev/null

# use printf to show path + size
find /fs/ess/PAS2880/users/kolganovaanna -type f -name 'ERR10802885_R1.fastq.gz' -printf '%p\t%k KB\n' 2>/dev/null

# test directly (no recursion)
ls -lh /fs/ess/PAS2880/users/kolganovaanna/garrigos-data/fastq/ERR10802885_R1.fastq.gz && echo "Found" || echo "Not found"
```

I think that this overview is sort of too broad. It offered a bunch of comamnds. Because I just want a path, I tested oinly the first 2 commands to see how they differ. The outputs were:

```bash
/fs/ess/PAS2880/users/kolganovaanna/garrigos-data/fastq/ERR10802885_R1.fastq.gz
/fs/ess/PAS2880/users/kolganovaanna/GA4/data/ERR10802885_R1.fastq.gz

/fs/ess/PAS2880/users/kolganovaanna/GA4/data
/fs/ess/PAS2880/users/kolganovaanna/garrigos-data/fastq
```
The outputs are sort of the same. 

'Find' is the command I wanted to use, but I would have just used 'find -name 'ERR10802885_R1.fastq.gz''. If I use this command this is my output:

```bash
./garrigos-data/fastq/ERR10802885_R1.fastq.gz
./GA4/data/ERR10802885_R1.fastq.gz
```

As you can see, my approach doesn't involve " 2>/dev/null" and I honeslty don't really understand what is the purpose of this code. Thus, I will ask Copilot a follow up question about it

Q2: # You provided this code "find /fs/ess/PAS2880/users/kolganovaanna -type f -name 'ERR10802885_R1.fastq.gz' 2>/dev/null". Please explain why what is the reason behind using 2>/dev/null in this command?

What Copilot answered:

```bash
2>/dev/null redirects the process's standard error (file descriptor 2) to the special device null (discarding it). It hides error messages (e.g., "Permission denied") so your output only shows successful results written to stdout.
```

The answer also included additional details, such as why use it with 'find', what it means (syntax break down), examples, cautions, and when I don't need it. Due to the large output, it makes more sense to include a screenshot of the answer rather that copy and paste it. Please see "Trial 1. Q2" file in GA5. 

I think that this explanation is okay but it took me a minute to understand it. I don't think we ever talked about stdout (I assume it's something like "standard output), so I asked Copilot another follow up question to clarify this.

Q3: # What is stdout? Leave out examples and other additional information and just explain to me what it means 

What Copilot answered: 

```bash
stdout (standard output) is the default stream a program uses to write its normal output.wants you to see — like the program talking to you.
```

*Conclusion*: I think that Copilot tended to overthink with this question. I believe that the answers it gave for Q1 were overwhelming and there are less comlicated ways to use "find". I'm pretty sure that we never needed '2>/dev/null'. Its answers to other questions were fine, at least I was able to understand them, however, not without further clarifications. If there was a scale I would give it 5/10. This is because if a person didn't know anything about coding, Copilot would have overcomplicated even such simple commands as 'find'. Although, I do understand that my prompt didn't say specify my coding experience level.

At the end, I committed to README.md

```bash
git add README.md
git commit -m "Trial 1"
```

*Trial 2*

Q1: What command should I use to copy files from garrigos-data/fastq into GA5/data (the directory already exists)

What Copilot answered:

```bash
Use rsync (recommended) — fast, resumable, preserves attributes. If you are inside GA5 run:

rsync -avh --progress ../garrigos-data/fastq/ data/

If you prefer plain cp, from GA5 run:

cp -av ../garrigos-data/fastq/. data/

Notes (brief):

The trailing slash on fastq/ copies the contents into data/.
Add -n to cp or --ignore-existing to rsync to avoid overwriting existing files.
```

I've never used rsync. I understand that if I run one of the commands it will copy the files into GA5/data and the other command won't work. Thus, I choose to use what I used before -'cp'. The output was:

```bash
'../garrigos-data/fastq/./ERR10802885_R1.fastq.gz' -> 'data/./ERR10802885_R1.fastq.gz'
'../garrigos-data/fastq/./ERR10802885_R2.fastq.gz' -> 'data/./ERR10802885_R2.fastq.gz'
'../garrigos-data/fastq/./ERR10802886_R1.fastq.gz' -> 'data/./ERR10802886_R1.fastq.gz'
'../garrigos-data/fastq/./ERR10802886_R2.fastq.gz' -> 'data/./ERR10802886_R2.fastq.gz'
```

It accomplished what I wanted. If I didn't use Copilot, I would have run this command:

```bash
cp -rv ../garrigos-data/fastq data
```

I can see that the difference between my approach and what has been offered by Copilot is really just that my command copys the "fastq" folder into GA5 directory. This is not necesserily a problem, but in this case I think that the option produced by Copilot is better. I do want to ask it about the first command it offered and see if it can explain it to me.

Q2: # Explain this command simply and shortly rsync -avh --progress ../garrigos-data/fastq/ data/

What Copilot answered:

```bash
Quick plain explanation:

Runs rsync to copy files from fastq into data/.
-a = archive mode (recursive + preserve permissions, timestamps, symlinks, etc.).
-v = verbose (shows what’s copied).
-h = human-readable sizes in output.
--progress = show per-file progress during transfer.
Trailing slash on the source (fastq/) means “copy the contents of this folder into data/” (not the folder itself).
```
This explanation makes sense to me. 

*Conclusion*: I think Trial 2 had Copilot performing better than in the case of Trial 1. I think that the command it gave to copy the files were better than what I used previously. This is a 9/10 (rsync was complicated to break down without help)

At the end, I committed to README

```bash
git add README.md
git commit -m "Trial 2"
```


*Trial 3*

Q1: # I would like to see if any of my fastq.gz file in GA5/data have a poly-A problem 

I don't know how to detect poly-A, but I did notice that this can be an issue while doing GA 4. This is why I am asking about it. 

What Copilot answered:

```bash
Good — here are compact, ready-to-run checks you can use from inside GA5 to detect files in data/ that have many reads ending in a long poly-A tail.

Quick scan (reports filename, total reads, poly-A reads, percent). This checks for reads with >=10 A's at the 3' end:

for f in data/*.fastq.gz; do
  zcat "$f" | awk -v fn="$f" '
    NR%4==2 { seq=toupper($0); total++; if (seq ~ /A{10,}$/) poly++ }
    END { printf "%s\t%d\t%d\t%.2f%%\n", fn, total, poly, (total?100*poly/total:0) }
  '
done

Same, but only print files where >= 1% of reads have that poly-A tail (adjust threshold as needed):

THRESH=1
for f in data/*.fastq.gz; do
  zcat "$f" | awk -v fn="$f" -v th="$THRESH" '
    NR%4==2 { seq=toupper($0); total++; if (seq ~ /A{10,}$/) poly++ }
    END { pct=(total?100*poly/total:0); if (pct >= th) printf "%s\t%d\t%d\t%.2f%%\n", fn, total, poly, pct }
  '
done

If you want to detect poly-A anywhere in the sequence (not just at the 3' end), use /A{10,}/ instead of /A{10,}$/:

# detect internal or terminal poly-A runs of length >=10
for f in data/*.fastq.gz; do
  zcat "$f" | awk -v fn="$f" 'NR%4==2{seq=toupper($0); total++; if(seq ~ /A{10,}/) poly++} END{printf "%s\t%d\t%d\t%.2f%%\n", fn, total, poly, (total?100*poly/total:0)}'
done

If files are large and you want a fast sample-based check (first 10000 reads):
SAMPLE=10000
for f in data/*.fastq.gz; do
  zcat "$f" | awk -v fn="$f" -v S="$SAMPLE" 'NR%4==2{seq=toupper($0); total++; if(seq ~ /A{10,}$/) poly++; if(total>=S) exit} END{printf "%s\t%d\t%d\t%.2f%% (sample)\n", fn, total, poly, (total?100*poly/total:0)}'
done

Notes / tuning

Change the poly-A length threshold (10) to a different value if you want to detect shorter/longer tails.
Use /A{10,}$/ to detect terminal poly-A at the 3' end, /A{10,}/ to detect any long A-run anywhere.
zcat streams the uncompressed reads so the commands don't create intermediate files.
Percentages are (poly-A reads) / (total reads) * 100.
If you want, I can run the scan now and show results, or produce a single summary table sorted by percent poly-A. Which would you like?
```

This is a lot of options. I think that it makes the most sense for me to use option 1 because I simply want to check for poly-A and since I'm not very familiarr with it, it doesn't make much sense to use other more specific commands. I ran option 1 and this was the output:

```bash
data/ERR10802885_R1.fastq.gz    500000  1592    0.32%
data/ERR10802885_R2.fastq.gz    500000  2065    0.41%
data/ERR10802886_R1.fastq.gz    500000  912     0.18%
data/ERR10802886_R2.fastq.gz    500000  915     0.18%
```

This output is really easy to understand because Copilot already explained the loop itself and its output. It seems like none of the reads in any of the files have a poly-A issue detected in more than 1% of the total. Interestingly, it offers to check specifically for >=1%, which makes we wonder if this is some sort of a thershold. I will ask Copilot this:

Q 2: # If poly-A tails represent more than >=1% of the reads, does it mean this is a significant problem? Or what is the reason behind you proposing to test specifically for >=1% in option 2?

The answer that Copilot gave me made sense but it is large. Therefore, I will show it as a screenshot. Please see file "Trial 3. Q2".

*Conclusion*: Copilot did great with this trial. Jelmer already explained to me some things about poly-A,  ut I feel like Copilot improved my understanding. 10/10 experience rate. 

At the end, I committed to README:

```bash
git add README.md
git commit -m "Trial 3"
```

*Trial 4* 

In this final trial, I want Copilot to write a code to test for both poly-A and poly-G in the fastq files while saving the outputs in a separate file under results/. I will have it make this directory on its own. I would also expect it to create a .sh file with the script. this is something that I only partially know how to do, I am mostly interested in seeing the combination for poly-A and poly-G testing and testing Copilot's logic of creating files. Let's see what it will do.

Q1 : # Write a script to test each file in GA5/data/ for poly-A and poly-G issue simultaneously. Print the output into results/trial4.txt

What Copilot answered:

```bash
I'll (1) add a concise todo entry for the script, (2) create the script file scripts/check_poly.sh, and (3) run it to produce results/trial4.txt and show the output head. I'll update the todo list when done.

/fs/ess/PAS2880/users/kolganovaanna/GA5/scripts/check_poly.sh

head -n 20 results/trial4.txt || true

Progress update — I added and ran a script that scans all gzip FASTQ files in data/ for poly-A and poly-G problems and wrote the results to trial4.txt.

What I changed

Added scripts/check_poly.sh (creates trial4.txt with columns: file, total_reads, polyA_count, polyA_pct, polyG_count, polyG_pct).
File path: check_poly.sh
It scans data/*.fastq.gz using zcat + awk and outputs percentages with four decimal places.
What I ran

Executed the script; it produced trial4.txt.
Shown the file header (first 20 lines). If you want, I can show the full file or a sorted summary.

How you can run it yourself
From inside GA5:

bash scripts/check_poly.sh
# then view results:
head -n 50 results/trial4.txt

Notes and next steps

The script currently detects runs of >=10 A or >=10 G at the 3' end (adjust regex /A{10,}/or/G10,/or/G10,/ if you want different thresholds).
If you want the script to be executable directly, run:

chmod +x scripts/check_poly.sh
./scripts/check_poly.sh

I can:
Tweak thresholds (length, internal vs terminal),
Produce a sorted table (e.g., by percent poly-A or poly-G),
Sample-only mode (first N reads) for speed,
Or include trimming recommendations and sample reads for inspection.
Which follow-up would you like?
```

I ran all of the commands except the last one - 'chmod'
The outputs were:

```bash
Wrote results to results/trial4.txt
file    total_reads     polyA_count     polyA_pct      polyG_count      polyG_pct
data/ERR10802885_R1.fastq.gz    500000  1592    0.3184 568      0.1136
data/ERR10802885_R2.fastq.gz    500000  2065    0.4130 5784     1.1568
data/ERR10802886_R1.fastq.gz    500000  912     0.1824 789      0.1578
data/ERR10802886_R2.fastq.gz    500000  915     0.1830 4475     0.8950
```

I saw how both check_poly.sh appeared in the new scripts/ directory and how the new trial4.txt file appeared under results/. If you check this .txt you'll see the exact same output as it provided in the terminal. I think Copilot did a good job explaining what it has run and it produced the output I more or less expected it to. In my second question, I want it to help me interpret this table it produced:

Q2: # What conclusions can I draw based on this output?

```bash
Summary of results

Files scanned: 4 (paired runs for two samples).
All files have 500,000 reads (total_reads column).
Poly‑A (≥10 A at 3' end):
ERR10802885_R1: 1,592 reads → 0.3184%
ERR10802885_R2: 2,065 reads → 0.4130%
ERR10802886_R1: 912 reads → 0.1824%
ERR10802886_R2: 915 reads → 0.1830%
Poly‑G (≥10 G at 3' end):
ERR10802885_R1: 568 reads → 0.1136%
ERR10802885_R2: 5,784 reads → 1.1568%
ERR10802886_R1: 789 reads → 0.1578%
ERR10802886_R2: 4,475 reads → 0.8950%
Conclusions you can draw

Overall levels of terminal poly‑A are low across all files (<0.5%). That is generally not concerning for most RNA‑seq workflows.
There is a notable poly‑G signal in two files:
ERR10802885_R2.fastq.gz shows ~1.16% poly‑G reads (5,784/500k) — above the 1% heuristic and worth investigating.
ERR10802886_R2.fastq.gz shows ~0.90% poly‑G reads — borderline, worth a quick look.
Poly‑G higher in R2 files suggests the issue may be read‑end or run-specific (e.g., chemistry/adapter/flowcell problem affecting the reverse reads) rather than sample‑specific polyadenylation.
```

I am more than ok with this explanation.

*Conclusion*: Despite the overwhelming amount of outputs and commands suggested, I think Copilot did everything right and it was able to come up with a way  to show the results most efficiently (a table). It did a good job explaining what the outputs mean and what potential problems (poly-G) we have. Probably the only issue I had with Copilot here is the complexity of the script in check_poly.sh file, which would have taken me a while to break down if I wanted to. 

I committed to README and check_poly.sh here:

```bash
git add scripts/check_poly.sh README.md
git commit -m "Trial 4"
```

5. Create a repository on GitHub, connect it to your local repo, and push your local repo to GitHub.

```bash
git add README.md
git commit -m "final commit"
git branch -M main
git remote add origin git@github.com:kolganovaanna/GA5.git
git push -u origin main
```