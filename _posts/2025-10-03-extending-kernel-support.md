---
layout: single
classes: wide
sidebar:
    nav: "navi"
title: "Extending kernel support with Virtme-ng"
date: 2025-10-03
---

## Introduction

If you’re working in a virtualized environment or with Linux kernels and filesystems, you know how crucial it is to automate and validate tests to ensure things are working smoothly. That’s where **Virtme-ng** comes into play. 

In this blog post, I’ll walk you through a recent project I worked on, where I developed a virtualized testing framework for **CernVM-FS (Cern Virtual Machine File System)**. This post will give you a semi-technical overview of the steps I took, the tools I used, and the challenges I overcame.

---

## Project Overview

In the project, I aimed to build a robust testing framework leveraging a powerful virtualization tool to extend the current test framework to multiple linux kernels. CernVM-FS relies on various kernel level syscalls and dependencies like libfuse, which undergo changes across the development of linux. This project was a successful attempt of developing a framework to catch issues in CernVM-FS due to the underlying kernel. The project evolved over 16 weeks, and I had the opportunity to experiment with various technologies and techniques.

Here’s a brief breakdown of what was achieved:

- **Phase 1**: A git bisect friendly setup using virtme-ng to pinpoint a kernel introducing a regression
- **Phase 2**: A virtualized test framework capable of running selected tests across a range of linux kernels
- **Phase 3**: A setup for using user selected libfuse version to aid in the above framework
  
---

## Tools & Technologies Used

I relied on several tools and technologies throughout the development process:

- [**Linux kernel**](https://www.kernel.org/){:target="_blank"}
- [**Virtme-ng**](https://github.com/arighi/virtme-ng){:target="_blank"}
- [**libFUSE**](https://github.com/libfuse/libfuse){:target="_blank"}
- [**VirtioFS**](https://gitlab.com/virtio-fs/virtiofsd){:target="_blank"}
- [**systemd**](https://systemd.io/){:target="_blank"}
- [**CMake**](https://cmake.org/){:target="_blank"}
- **bash**

---

## Key Features & Functionality

Here are some key features of the project that I implemented:

### Automated Kernel & Filesystem Testing

The main goal of VNG Testing is to run automated tests that validate kernel functionality in a controlled environment. It ensures that changes to the kernel or filesystem don't break anything, and that the system remains stable and secure.

### Virtualized Testing Environment

Using **virtme-ng** at the heart of the framework, the tests run inside a lightweight virtual machine (VM), providing complete isolation from your primary system. The tests can be run with different configurations or kernel versions, ensuring compatibility and functionality across various setups.

### Custom libfuse integration

As part of the virtualized testing framework, I integrated the ability to use a custom version of libfuse for testing. This was critical because CernVM-FS relies heavily on the libfuse library for filesystem operations, and different versions can introduce subtle regressions or incompatibilities. By enabling developers to specify a custom libfuse version during test execution, I ensured that the framework could handle various version-specific edge cases. This added another layer of flexibility, allowing the tests to be more comprehensive and adaptable to evolving dependencies.

### Test Extensibility

Adding new tests or modifying existing ones is easy. VNG Testing is designed to be flexible, allowing you to create new test suites or update current ones with minimal effort.

---

## Challenges Faced

No project is without its challenges. Some of the key obstacles I faced during development were:

- **Challenge 1**: Adjusting the virtual environment to run CernVM-FS client and server as they would on a normal host
- **Challenge 2**: Managing mounts inside the virtual machine to be used by CernVM-FS
- **Challenge 3**: Optimizing the virtual machine setup to run tests efficiently while maintaining sufficient isolation for accurate results
- **Challenge 4**: Overcoming a limitation in virtme-ng to run systemd fstab generator which was crucial for running CernVM-FS server

To overcome these, I researched various solutions like creating tmpfs mounts for quick IO, enabling systemd support in virtme-ng by using /dev/null etc. and eventually, I was able to implement a strategy that worked effectively.

---

## Lessons Learned

Throughout this project, I learned a lot, both from a technical and personal standpoint:

- **Lesson 1**: Flexibility is key in automated testing. Tools like virtme-ng can help scale testing across multiple environments, but ensuring flexibility in your framework (e.g., through custom libfuse) helps tackle version-specific issues
- **Lesson 2**: Proper environment isolation is crucial when testing low-level system components like filesystems. Using a VM allows for greater stability in testing
- **Lesson 3**: Well-tested software is reliable software. It’s important to prioritize test extensibility early in development. This makes adding future tests and troubleshooting much easier and builds a robust system
- **Lesson 4**: I gained more depth in kernel modules, systemd and how various other libraries integrate with the kernel to give user the ability to perform operations
- **Lesson 5**: Last but most importantly, Documentation is equally important as development as it provides a clear understanding of the software's functionality

These lessons will undoubtedly help me in future projects and improve my approach to development.

---

## Pull Requests

You can check out the details of the changes I made via the following pull requests:

- [**Virtme-ng integration with CernVM-FS**](https://github.com/cvmfs/cvmfs/pull/3986){:target="_blank"}
- [**Using custom libFUSE version with CernVM-FS**](https://github.com/cvmfs/cvmfs/pull/3998){:target="_blank"}
- [**Vendoring libFUSE by utilising CMake build process**](https://github.com/cvmfs/cvmfs/pull/3996){:target="_blank"}
- [**Git Bisect setup to pinpoint kernel regressions**](https://github.com/ShivamMadlani/CVMFS_kernel_test){:target="_blank"}

This includes improvements to the test framework, integration of custom libfuse versions, and optimizations for kernel compatibility.

---

## Conclusion

In conclusion, this project has been a rewarding experience. I was able to apply my skills and knowledge to build something from the ground up, tackle challenges, and deliver a product that met the initial goals. 

I hope this overview has given you a good sense of the project and the steps I took to complete it. Feel free to reach out if you have any questions or want to dive deeper into any aspect of the project!

---
