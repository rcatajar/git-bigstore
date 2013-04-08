git-bigstore
============

git-bigstore is an extension to Git that helps you track big files. For technical details, check out the [Wiki](https://github.com/aurorasoftware/git-bigstore/wiki/Bigstore).

Configuration
-------------

To get started, set up an Amazon S3 bucket to store stuff. Once you have your access key id and secret access key on hand, run the following:

    $ pip install git-bigstore
    $ git bigstore init
    What backend would you like to store your files with?
    (1) Amazon S3
    (2) Google Cloud Storage
    (3) Rackspace Cloud Files
    Enter your choice here: 1

    Enter your Amazon S3 Credentials

    Access Key: key
    Secret Key: secret
    Bucket Name: my-bucket

Well, that was easy! Your Git repository is now prepared to track big files. If a ".bigstore" configuration file already exists in your repository, you will not be prompted for backend credentials.

To specify filetypes to store remotely, add an entry to your .gitattributes. E.g., if you only want to store your big archive files in S3, run this command in your repository root:

    $ echo "*.zip filter=bigstore" > .gitattributes

After you run this, every time you stage a zip file, it will transparently copy the file to ".git/bigstore/objects" and will replace the file contents (as stored in git) with relevant identifying information.

git-bigstore won't automatically sync to S3 after a commit. To push changed files, just run:

    $ git bigstore push

To pull down remote changes:

    $ git bigstore pull


But "INSERT X HERE" already exists...
---------------------------------

I've been using git-media for a few days now, and I've observed that it breaks down because it violates the following guideline in the [Git docs](https://www.kernel.org/pub/software/scm/git/docs/gitattributes.html):

> For best results, clean should not alter its output further if it is run twice ("clean→clean" should be equivalent to "clean"), and multiple smudge commands should not alter clean's output ("smudge→smudge→clean" should be equivalent to "clean").

This made it a bit tough to collaborate with multiple people, since Git would try to clean things that had already been cleaned, and smudge things that had already been smudged. No good!

Also, git-media hasn't been updated in a while. I promise to be a good maintainer!

git-annex is another alternative, but it's solving a different problem and its implementation is a bit less dependent on Git itself. As a result, you essentially have to learn a whole new set of commands to work with it. I wanted to create something with as minimal complexity as possible.

Copyright
---------

Copyright 2013 Aurora Software LLC <hi@aurora.io>.

Licensed under Apache 2.0. See LICENSE for more details.

