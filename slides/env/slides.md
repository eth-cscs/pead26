---
theme: ./slidev-theme-cscs
---

## Software Environments at CSCS

Ben Cumming

eth-cscs.github.io/pead26

PEAD at CuG 2026

---
layout: two-cols
layoutClass: gap-2
---

# Alps

CSCS deploys **vCluster**s on a single HPE Cray EX system **Alps**
- 6 node types
- over 10 use-case specific production clusters

CSCS is migrating away from installing CPE on vClusters
- CPE is provided in containers
- It is still installed on one cluster where it is marked as deprecated

::right::

<br>
<br>
<br>

<div class="flex justify-center">
    <img src="./images/alps-blah.png" class="h-65" alt="Alt text for the image">
</div>


---
layout: two-cols-header
layoutClass: gap-2
---

# Containers and uenv

CSCS provides two ways to set up software environments:
* CSCS provides **uenv**.
* users can bring their own **containers**.

Both approaches allow normal users to build and deploy full stacks without root permission.

::left::

## uenv

* Use Spack to build self-contained environments
    * cray-mpich RPM is repackaged for Spack
    * libfabric is built-in or pulled from system
* Each env is a SquashFS file stored in a registry
* Lightweight CLI runner and SLURM plugin mount the SquashFS and set environment variables

<br>

::right::

## containers

* Use Podman to build containers
* Convert containers to SquashFS images
* SLURM plugin mounts and chroots
* Hooks extend containers with native network support, NCCL-aws-ofi plugin, GPUs (NVIDIA and AMD), etc.

---

# CPE Components Used

<div class="flex justify-center">
    <img src="./images/stack.png" class="h-65" alt="Alt text for the image">
</div>

Observation: Conway's law might explain the different deployment of `libfabric`+`libcxi` and `cray-mpich` and its dependencies.

---

# HPE are going in the right direction...

* Open source cray-mpich and its dependencies:
    * it is getting harder to support the matrix of `aarch64`x`x86_64`, `NVIDIA`x`AMD`x`CPU only`, `SP5`x`SP6`x`SP7`, `GNU`x`NVFortran`x`intel`x`LLVM` versions
    * CPE components are the last closed source user-facing software from HPE used in our stacks

* Commit to flexible deployment of packages:
    * parts of the stack are still designed to be released monolithically (e.g. libcxi + cxi-driver)
    * Integrate aws-ofi-nccl, nvshmem/openshmem into the support matrix
    * we lost access to the the "experimental" RPM repository for a few weeks with no explanation

* Support up-to-date GPU drivers
    * From the customer's perspective **_it is a CPE problem_**.


---

# HPE should avoid...

Handing off responsibility to support and develop any of the components required to take advantage of the HPE network special sauce.

I think things are generally going in the right direction - but slowly.

**HPE/Cray have always provided a better user-facing software experience**:
* How some centers want to deploy is at odds with the CPE deployment - don't lose your advantage.

---

# We are nearly there

An open source cray-mpich would allow us to install a complete HPE Cray EX optimised software stack using Spack from one configuration file.

I won't have to repackage RPMs or maintain tools like Stackinator.
