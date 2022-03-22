# Mayhem + GitHub Actions

In this exercise you will setup a GitHub Action that analyzes a toy project automatically everytime there is a push to the repository.

## Forking the repository

Before we begin, we first must fork the repository.

1. In your browser, navigate to https://github.com/makesoftwaresafe/mayhem-cmake-example.

2. Click "Fork" near the top of the page.

![Fork](assets/images/gh-click-fork.png)

3. When asked where you want to create the fork, select your username.

![Fork to Your Account](assets/images/gh-click-fork.png)

Now you have your own fork of the mayhem-cmake-example. Next, we'll clone and run the toy target locally.

## Clone and Run Locally

1. Copy the HTTP Clone URL using the button that can be found on your fork's GitHub page.

![Copy Fork URL](assets/images/gh-get-fork-url.png)

2. Back in a terminal, enter `git clone ` and paste the URL, then press enter. Your command should look something like this:

    ```
    git clone https://github.com/nathanjackson/mayhem-cmake-example.git
    ```

3. Once the clone completes, change into the directory.

    ```
    cd mayhem-cmake-example/
    ```

4. Now, create a build directory and change into it.

    ```
    mkdir build
    cd build/
    ```

5. Run cmake to generate the Makefile and build tree.

    ```
    CC=clang CXX=clang++ cmake ..
    ```

6. Run Make

    ```
    make
    ```

7. Once the build completes, try running the libFuzzer target.

    ```
    ./fuzzme
    ```

The target should eventually find a crash and present you with output that resembles this:

```
INFO: Seed: 2682090316
INFO: Loaded 1 modules   (7 inline 8-bit counters): 7 [0x4e8040, 0x4e8047), 
INFO: Loaded 1 PC tables (7 PCs): 7 [0x4be940,0x4be9b0), 
INFO: -max_len is not provided; libFuzzer will not generate inputs larger than 4096 bytes
INFO: A corpus is not provided, starting from an empty corpus
#2	INITED cov: 3 ft: 4 corp: 1/1b exec/s: 0 rss: 24Mb
#3	NEW    cov: 4 ft: 5 corp: 2/2b lim: 4 exec/s: 0 rss: 24Mb L: 1/1 MS: 1 ShuffleBytes-
#2769	NEW    cov: 5 ft: 6 corp: 3/3b lim: 29 exec/s: 0 rss: 24Mb L: 1/1 MS: 1 ChangeByte-
#19447	NEW    cov: 6 ft: 7 corp: 4/5b lim: 191 exec/s: 0 rss: 24Mb L: 2/2 MS: 3 InsertByte-ShuffleBytes-ChangeBinInt-
==37703== ERROR: libFuzzer: deadly signal
    #0 0x4aded0 in __sanitizer_print_stack_trace (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4aded0)
    #1 0x45a1d8 in fuzzer::PrintStackTrace() (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x45a1d8)
    #2 0x43f323 in fuzzer::Fuzzer::CrashCallback() (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x43f323)
    #3 0x7f6e0d50f3bf  (/lib/x86_64-linux-gnu/libpthread.so.0+0x143bf)
    #4 0x7f6e0d1d003a in __libc_signal_restore_set /build/glibc-sMfBJT/glibc-2.31/signal/../sysdeps/unix/sysv/linux/internal-signals.h:86:3
    #5 0x7f6e0d1d003a in raise /build/glibc-sMfBJT/glibc-2.31/signal/../sysdeps/unix/sysv/linux/raise.c:48:3
    #6 0x7f6e0d1af858 in abort /build/glibc-sMfBJT/glibc-2.31/stdlib/abort.c:79:7
    #7 0x4ae2e3 in fuzzme (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4ae2e3)
    #8 0x4ae370 in LLVMFuzzerTestOneInput (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4ae370)
    #9 0x4409e1 in fuzzer::Fuzzer::ExecuteCallback(unsigned char const*, unsigned long) (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4409e1)
    #10 0x440125 in fuzzer::Fuzzer::RunOne(unsigned char const*, unsigned long, bool, fuzzer::InputInfo*, bool*) (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x440125)
    #11 0x4423c7 in fuzzer::Fuzzer::MutateAndTestOne() (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4423c7)
    #12 0x4430c5 in fuzzer::Fuzzer::Loop(std::__Fuzzer::vector<fuzzer::SizedFile, fuzzer::fuzzer_allocator<fuzzer::SizedFile> >&) (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x4430c5)
    #13 0x431a7e in fuzzer::FuzzerDriver(int*, char***, int (*)(unsigned char const*, unsigned long)) (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x431a7e)
    #14 0x45a8c2 in main (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x45a8c2)
    #15 0x7f6e0d1b10b2 in __libc_start_main /build/glibc-sMfBJT/glibc-2.31/csu/../csu/libc-start.c:308:16
    #16 0x40681d in _start (/home/nathan/src/mayhem-cmake-example/build/fuzzme+0x40681d)

NOTE: libFuzzer has rudimentary signal handlers.
      Combine libFuzzer with AddressSanitizer or similar for better crash reports.
SUMMARY: libFuzzer: deadly signal
MS: 1 InsertByte-; base unit: 6721d9fb11ca730b528749d7e4f9e8c52766e450
0x62,0x75,0x67,
bug
artifact_prefix='./'; Test unit written to ./crash-6885858486f31043e5839c735d99457f045affd0
Base64: YnVn
```

Congratulations, you've just built a libFuzzer target using CMake! Now, let's try to automate this build process using our Dockerfile.

## Modify the Template Dockerfile

1. Modify the target and add our build commands.

2. Verify we can build locally.

3. Verify we can run locally.

## Add a Mayhemfile

1. Fill out "template" Mayhemfile.

## Setup your MAYHEM_TOKEN Repository Secret

1. Navigate to the settings page of your fork.

2. Expand the secrets drop down and select "Actions".

3. Click "New repository secret".

4. Under name enter "MAYHEM_TOKEN".

3. In another tab or window, open mayhem.forallsecure.com.

4. Copy your API Token

5. Paste into the "Value" field back in GitHub.

6. Click Add secret.

## Setup GitHub Action Workflow

1. Create .github/workflows directory.

2. Create a new mayhem.yml.

3. Copy the example from https://github.com/ForAllSecure/mcode-action-examples/blob/main/.github/workflows/mayhem.yml

4. Commit and Push to your fork.