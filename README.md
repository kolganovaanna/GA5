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

I think that this overview is sort of too broad. It offered a bunch of commands. Because I just want a path, I tested only the first 2 commands to see how they differ. The outputs were:

```bash
/fs/ess/PAS2880/users/kolganovaanna/garrigos-data/fastq/ERR10802885_R1.fastq.gz
/fs/ess/PAS2880/users/kolganovaanna/GA4/data/ERR10802885_R1.fastq.gz

/fs/ess/PAS2880/users/kolganovaanna/GA4/data
/fs/ess/PAS2880/users/kolganovaanna/garrigos-data/fastq
```
The outputs are sort of the same, except the first one contains the name of the file itself. 

'Find' is the command I wanted to use, but I would have just used 'find -name 'ERR10802885_R1.fastq.gz'. If I use this command, this is my output:

```bash
./garrigos-data/fastq/ERR10802885_R1.fastq.gz
./GA4/data/ERR10802885_R1.fastq.gz
```

As you can see, my approach doesn't involve " 2>/dev/null" and I honeslty don't really understand what is the purpose of this code line. Thus, I will ask Copilot a follow up question about it

Q2: # You provided this code "find /fs/ess/PAS2880/users/kolganovaanna -type f -name 'ERR10802885_R1.fastq.gz' 2>/dev/null". Please explain why what is the reason behind using 2>/dev/null in this command?

What Copilot answered:

```bash
2>/dev/null redirects the process's standard error (file descriptor 2) to the special device null (discarding it). It hides error messages (e.g., "Permission denied") so your output only shows successful results written to stdout.
```

The answer also included additional details, such as why use it with 'find', what it means (syntax break down), examples, cautions, and when I don't need it. Due to the large output, it makes more sense to include a screenshot of the answer rather that copy and paste it. Please see "Trial 1. Q2" file in GA5 directory. 

I think that this explanation is okay but it took me a minute to understand it. I don't think we ever talked about stdout (I assume it's something like "standard output"), so I asked Copilot another follow up question to clarify this.

Q3: # What is stdout? Leave out examples and other additional information and just explain to me what it means 

What Copilot answered: 

```bash
stdout (standard output) is the default stream a program uses to write its normal output.wants you to see — like the program talking to you.
```

*Conclusion*: I think that Copilot tended to overthink with in this trial. I believe that the answers it gave for Q1 were overwhelming and there are less complicated ways to use "find". I'm pretty sure that we never needed '2>/dev/null'. Its answers to other questions were fine, at least I was able to understand them, however, not without further clarifications. If there was a scale I would give it 5/10. This is because if a person didn't know anything about coding, Copilot would have overcomplicated even such simple commands as 'find'. Although, I do understand that my prompt didn't specify my coding experience level at all. 




*Trial 2*

Q1: # What command should I use to copy files from garrigos-data/fastq into GA5/data (the directory already exists)

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

I've never used rsync. I understand that if I ran one of the commands it will copy the files into GA5/data and the other command won't be necessary to use (and might not even work). Thus, I choose to use what I used before -'cp'. The output was:

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

I can see that the difference between my approach and what has been offered by Copilot is really just that my command copies the "fastq" folder into GA5 directory. This is not necesserily a problem, but in this case I think that the option produced by Copilot is better. I do want to ask it about the first command it offered and see if it can explain it to me.

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


*Trial 3*

I don't know how to detect poly-A, but I did notice that this can be an issue while doing GA 4. This is why this trial will focus on poly-A, as it is something I have no idea how to deal with. 


Q1: # I would like to see if any of my fastq.gz file in GA5/data have a poly-A problem 

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

This is a lot of options. I think that it makes the most sense for me to use option 1 because I simply want to check for poly-A and since I'm not very familiar with it, it doesn't make much sense to use other more specific commands. I ran option 1 and this was the output:

```bash
data/ERR10802885_R1.fastq.gz    500000  1592    0.32%
data/ERR10802885_R2.fastq.gz    500000  2065    0.41%
data/ERR10802886_R1.fastq.gz    500000  912     0.18%
data/ERR10802886_R2.fastq.gz    500000  915     0.18%
```

This output is really easy to understand because Copilot already explained the loop itself and what it does. It seems like none of the reads in any of the files have a poly-A issue detected in more than 1% of the total reads. Interestingly, it offers to check specifically for >=1%, which makes we wonder if this is some sort of a thershold. I will ask Copilot this:

Q 2: # If poly-A tails represent more than >=1% of the reads, does it mean this is a significant problem? Or what is the reason behind you proposing to test specifically for >=1% in option 2?

The answer that Copilot gave me made sense but it is large. Therefore, i will show it as a screenshot.



