# About 'craper'

<p align="center">
   </a>
      <a href="https://github.com/Gh0stAn0n/packdoor">
      <img src="https://img.shields.io/badge/Version-2.0.0-darkgreen">
        <img src="https://img.shields.io/badge/Release%20Date-august%202023-purple">
  <img src="https://shields.io/badge/Python-100%25-066da5">
  <img src="https://shields.io/badge/Platform-Linux/Windows/Mac-darkred">
    </a>
  </p>
</p>

craper (courses scraper) is a Python script that allows users to search and retrieve free courses links from various websites.

The script uses web scraping techniques to extract relevant course information. The user can provide search queries as arguments to find the courses of interest.

### Project features:

Searches for free available courses on the following websites: "freecoursesite.com," "gigacourse.com," and "courseclub.me."

Retrieves course details, including URL and content size (in GB or MB).

Supports multi-threading for faster search and loading animation.

### Usage:

Ensure you have Python 3 or more installed.

Install required dependencies: pip install requests beautifulsoup4.

Run the script with search queries as arguments.

    python3 craper.py Matlab
    python3 craper.py rust-and-crust


### Search multiple queries:

Add manually the wanted keywords into a file by using for example: echo -n "devops\ndevseops" > file

    id=1; cat file | while read argv; do if [[ $id -lt 10 ]]; then id=0$id; fi; echo "$id : $argv"; python3 craper.py $argv >> ALL; ((id++)) ;done

![2024-04-11 16_27_51-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/523443f6-d65c-4ed9-a353-cbe3d16f7276)


### File Output:

After mannually removing the unwanted data (garbage generated by the loading sign) from the file:

![2024-04-11 16_44_07-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/55144780-5c01-4f11-8f09-1496881bd022)

Or by using REGular EXpressions:

![2024-04-11 16_44_29-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/d1be06f9-7116-466d-bb16-a733e5d617e5)


### Filter using REGEX:

The courses may be available in multiple domain with a different filename in the URI.
In  order to sort it and retrieve the most potential valuable (based on their mentions), use sort and uniq command combined with regex.

    cat ALL | grep -iE https | cut -d "/" -f4 | sed 's/^[0-9]*-//' | sed 's/-[0-9]*$//' | sort -n | uniq -c | sort -u

![2024-04-15 11_32_26-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/415ce0b4-b95c-45d5-aef9-29e4145a868a)

Finally, grep the filename from the original file to retrieve the course URL and its size amount.

Get the line number of the wanted course:

    cat ALL | grep -n learn-devops-ci-cd-with-jenkins-using-pipelines-and-docker
Increase the line number to get nothing but the URL and size amount of the course.

    cat ALL | grep -n learn-devops-ci-cd-with-jenkins-using-pipelines-and-docker | cut -d ":" -f1 | while read num; do ((num++)); echo $num; done
Get the full result and compare the differences between each link and VOILA.

    cat ALL | grep -n learn-devops-ci-cd-with-jenkins-using-pipelines-and-docker | cut -d ":" -f1 | while read num; do ((num++)); cat ALL | head -$num | tail -2; echo ""; done

![2024-04-15 12_27_54-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/e54753d8-94e6-4e5e-beb2-c3aaaff8bca1)

### Search for multiple queries and their URL

You can either do it based on the most common URI using tail:

    cat ALL | grep -iE https | cut -d "/" -f4 | sed 's/^[0-9]*-//' | sed 's/-[0-9]*$//' | sort -n | uniq -c | sort -u | tail | cut -d " " -f8 | while read uri; do echo "\n\nCOURSE: $uri"; cat ALL | grep -n "$uri" | cut -d ":" -f1 | while read num; do ((num++)); cat ALL | head -$num | tail -2; echo ""; done; done
![2024-04-15 12_45_19-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/87fec506-7cf1-4004-9e05-9576308cb438)

Or do it mannually, using the filenames itself instead of tail:

    cat ALL | grep -iEn (learn-devops-ci-cd-with-jenkins-using-pipelines-and-docker|the-complete-devsecops-course-with-docker-and-kubernetes) | cut -d ":" -f1 | while read num; do ((num++)); cat ALL | head -$num | tail -2; echo ""; done
![2024-04-15 13_31_08-Kali Linux  Running  - Oracle VM VirtualBox](https://github.com/Gh0stAn0n/craper/assets/102325071/b5dcdd02-0552-428b-8aa4-b263abbc9566)


### Updates:

Update 2.0.0 allows the user to search for symbols inside the query.

If C++ was searched before, it would have interfered with the URL, causing it to give false-positive results.

The allowed symbols in the current craper version are: #, *, +, -

### Notes:

This script may rely on the structure of the target websites, so changes to those websites' layouts may affect the script's functionality.
Ensure you comply with the terms of use of the websites being scraped and respect their policies.
The previous mentionned websites propose free courses that were illegally downloaded. using illegal content may be tempting, but it's important to remember that it's against the law and can have serious consequences.
