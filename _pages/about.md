---
permalink: /
title: "About me"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

Hi! I am Yisen Xu(许宜森). I am a master student of [School of Computer Science](http://cs.whu.edu.cn/), [Wuhan University](https://www.whu.edu.cn/), advised by Dr.[Jifeng Xuan](http://www.jifeng-xuan.com/). Early on, I received my bachelor degree at School of Computer Science, Wuhan University, majoring in Software Engineering. My research interests lie within software testing, machine learning, nature language learning and deep learning. 

Publications
======

* Yuhang Li, Shi Ying, Xiangyang Jia, **Yisen Xu**, Lili Zhao, Guoli Cheng, Bingming Wang, Jifeng Xuan. "EH-Recommender: Recommending Exception Handling Strategies Based on Program Context". Proceedings of the 23rd IEEE International Conference on Engineering of Complex Computer Systems (ICECCS 2018), Melbourne, Austrilia, December 12-14, 2018.
* **Yisen Xu**, Xiangyang Jia, Jifeng Xuan. "Writing Tests for This Higher-Order Function First: Automatically Identifying Future Callings to Assist Testers". Proceedings of the 11th Asia-Pacific Symposium on Internetware (Internetware 2019), Fukuoka, Japan. 2019.
* **Yisen Xu**, Fan Wu, Xiangyang Jia, Lingbo Li, Jifeng Xuan. "Mining the Use of Higher-Order Functions: An Exploratory Study on Scala Programs". Proceedings of the National Software Application Conference (NASAC 2019), Hangzhou, China. 2019.

Awards & Honors
======

* First Prize Scholarship, Wuhan University, 2018
* Third Prize of [Citi Financial Innovation Application Competition](http://www.citigroup.com/china/csts/EducationProgram/AboutEducation_CFCcn.html), 2017
1. Register a GitHub account if you don't have one and confirm your e-mail (required!)
1. Fork [this repository](https://github.com/academicpages/academicpages.github.io) by clicking the "fork" button in the top right. 
1. Go to the repository's settings (rightmost item in the tabs that start with "Code", should be below "Unwatch"). Rename the repository "[your GitHub username].github.io", which will also be your website's URL.
1. Set site-wide configuration and create content & metadata (see below -- also see [this set of diffs](http://archive.is/3TPas) showing what files were changed to set up [an example site](https://getorg-testacct.github.io) for a user with the username "getorg-testacct")
1. Upload any files (like PDFs, .zip files, etc.) to the files/ directory. They will appear at https://[your GitHub username].github.io/files/example.pdf.  
1. Check status by going to the repository settings, in the "GitHub pages" section

Site-wide configuration
------
The main configuration file for the site is in the base directory in [_config.yml](https://github.com/academicpages/academicpages.github.io/blob/master/_config.yml), which defines the content in the sidebars and other site-wide features. You will need to replace the default variables with ones about yourself and your site's github repository. The configuration file for the top menu is in [_data/navigation.yml](https://github.com/academicpages/academicpages.github.io/blob/master/_data/navigation.yml). For example, if you don't have a portfolio or blog posts, you can remove those items from that navigation.yml file to remove them from the header. 

Create content & metadata
------
For site content, there is one markdown file for each type of content, which are stored in directories like _publications, _talks, _posts, _teaching, or _pages. For example, each talk is a markdown file in the [_talks directory](https://github.com/academicpages/academicpages.github.io/tree/master/_talks). At the top of each markdown file is structured data in YAML about the talk, which the theme will parse to do lots of cool stuff. The same structured data about a talk is used to generate the list of talks on the [Talks page](https://academicpages.github.io/talks), each [individual page](https://academicpages.github.io/talks/2012-03-01-talk-1) for specific talks, the talks section for the [CV page](https://academicpages.github.io/cv), and the [map of places you've given a talk](https://academicpages.github.io/talkmap.html) (if you run this [python file](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.py) or [Jupyter notebook](https://github.com/academicpages/academicpages.github.io/blob/master/talkmap.ipynb), which creates the HTML for the map based on the contents of the _talks directory).

**Markdown generator**

I have also created [a set of Jupyter notebooks](https://github.com/academicpages/academicpages.github.io/tree/master/markdown_generator
) that converts a CSV containing structured data about talks or presentations into individual markdown files that will be properly formatted for the academicpages template. The sample CSVs in that directory are the ones I used to create my own personal website at stuartgeiger.com. My usual workflow is that I keep a spreadsheet of my publications and talks, then run the code in these notebooks to generate the markdown files, then commit and push them to the GitHub repository.

How to edit your site's GitHub repository
------
Many people use a git client to create files on their local computer and then push them to GitHub's servers. If you are not familiar with git, you can directly edit these configuration and markdown files directly in the github.com interface. Navigate to a file (like [this one](https://github.com/academicpages/academicpages.github.io/blob/master/_talks/2012-03-01-talk-1.md) and click the pencil icon in the top right of the content preview (to the right of the "Raw | Blame | History" buttons). You can delete a file by clicking the trashcan icon to the right of the pencil icon. You can also create new files or upload files by navigating to a directory and clicking the "Create new file" or "Upload files" buttons. 

Example: editing a markdown file for a talk
![Editing a markdown file for a talk](/images/editing-talk.png)

For more info
------
More info about configuring academicpages can be found in [the guide](https://academicpages.github.io/markdown/). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.
