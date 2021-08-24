---
# Please do not delete --- above :)

# ========================================================================
# Automatically edited by GitHub actions, please do not modify     | START
# ==============================================================
report_url: https://github.com/htwangtw/2021_08_Wang_load-confounds

last_changed: 2021-08-24 20:26 UTC

auth_created: htwangtw
# ===============================================================  | END

# Name of the event.
# -----------------
event: 'Brainhack Global 2020 Montreal'

# The title of your report.
# -------------------------
title:  'Load fMRIprep confounds in Python'

# Please SET this url to your GitHub project repo!
# -------------------------------------------------
url: https://github.com/htwangtw/2021_08_Wang_load-confounds

# Please edit affiliation details. The affiliation
# ids defined here will be used in the author list.
# -------------------------------------------------
affiliations:
- id: criugm
  orgname: 'CRIUGM'
  street: '4565 chemin Queen Mary'
  postcode: 'H3W 1W4'
  city: Montreal
  state: Quebec
  country: Canada
- id: bsms
  orgname: 'Brighton & Sussex Medical School'
  street: 'Trafford Centre'
  postcode: 'BN1 9RX'
  city: Brighton
  state: East Sussex
  country: UK
- id: aff2
  orgname: 'name'
  street: 'street'
  postcode: 'code'
  city: city
  state: state
  country: country

# Please list the authors.
# If an author has multiple affiliations, please
# add corref field and indicate the primary affiliation.
# -----------------------------------------------------
author:
- initials: PB
  surname: Bellec
  firstname: Pierre
  email: pierre.bellec@criugm.qc.ca
  affiliation: criugm
- initials: HTW
  surname: Wang
  firstname: Hao-Ting
  email: wang.hao-ting@criugm.qc.ca
  affiliation: criugm, bsms
  corref: criugm
  # Please make sure that you set corref (corresponding aff) if you have
  # multiple afiliations

# Please write a brief summary of your project.
# This abstract will (only) appear on the webpage.
# -------------------------------------------------
summary: load_confounds is a tool for loading a sensible subset of the fMRI confounds generated with fMRIprep in python (Esteban et al., 2019). The outputs can be directly passes to nilearn NifitMasker for denoising. The aim at Brainhack MTL 2020 is to implement new strategies as well as imporving the existing functions and documentations for a potential Beta release.

# Please add 1 to 3 tags
tags:
  - fmriprep
  - confounds
  - nilearn

# Please comment out the following 5 lines if you have no supplemental material.
# supplemental:
#   - name: Material 1
#     url: https://osf.io
#   - name: Material 2
#     url: https://zenodo.org

coi: None

acknow: The authors would like to thank the organizers and attendees of Brainhack Global Montreal 2020.

contrib: HTW wrote the software, HTW wrote the report.

# Please comment out the following 4 lines if no reviewer has been assigned to you yet.
# reviewers:
#   - name: Agah
#     surname: Karakuzu
#     gh_handle: agahkarakuzu

# Show/hide the BinderHub (mybinder.org) badge
# Accepted values: true/false (case sensitive)
# -------------------------------------------
binder: true

# Enable/disable hypothes.is
# Accepted values: true/false (case sensitive)
hypothesis: true

# Please do not delete --- below :)
---

# Introduction

fMRIprep \cite{fmriprep:2019} is a popular minimal preprocessing software for functional MRI data.
'Minimal preprocessing' refers to motion correction, field unwarping, normalization, bias field correction, and brain extraction.
Confound regression and smoothing are exculded from the workflow.
Instead, fMRIprep provides users with a large set of potential confound regressors that covers many denoising strategies.
The users will have to select the confound regressors for denoising in the subsequent analysis.
Loading a sensible subset of confounds is difficult and error prone for many strategies, such as ICA-AROMA \cite{icaaroma:2015} and CompCor \cite{compcor:2007}.
\code{load\_confounds} can access confound variables and provides preset strategies for confound selections.
The loaded format is competible with \code{nilearn} analysis functions such as \code{NiftiMasker} and the GLM modules.
The aim is to provide a easy and foolproof API for users to perform subsequent denoising of \code{fMRIprep} output.

# Progress

At Brianhack Global Montreal 2020, the aim is to prepare the package ready for a potential Beta release.
To prepare for the release, related issues involves completing the strategies missing and improve the user experience with better examples and error messages.
Several issues has been identified before Brainhack and the full discussion can be found under \code{load\_confounds} \href{https://github.com/SIMEXP/load_confounds/issues?q=is%3Aissue+label%3AbrainhackMTL2020+is%3Aclosed}{GitHub issue page}.
Participants at Brainhack were encouraged to pick up existing issues from the list.
The following issues have been discussed and/or resolved:

## Strategies

We worked on three strategies:
\begin{itemize}
  \item Added ICA-AROMA \cite{icaaroma:2015} (contributed by Hao-Ting Wang)
  \item Added Scrubbing \cite{power:2012}(contributed by Steven Meisler)
  \item Improved the anatomical mask selection for anatomical CompCor \cite{compcor:2007} (contributed by Steven Meisler)
\end{itemize}

## Demo

An executable demo using the nilearn developmental fMRI dataset (\href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228}) was added (contributed by Michael W. Weiss).
The demo was adapted from an exisitng nilearn example on denoising \href{https://nilearn.github.io/auto_examples/03_connectivity/plot_signal_extraction.html#sphx-glr-auto-examples-03-connectivity-plot-signal-extraction-py}{'Extracting signals from a brain parcellation'}.
This example notebook show how to extract signals from a brain parcellation and compute a correlation matrix, using different denoising strategies using the package.

## Error message

New class \code{NotFoundConfoundException} was added for collecting all the missing parameter needed for the given noise component(s).
A final error would be raised with the list of all parameters missing, rather than just the first encountered missing parameter. (Contributed by Michael W. Weiss and François Paugam)

## Identify test dataset

\href{https://openneuro.org/datasets/ds003}{OpenNeuro ds003} is adopted as the new test data, including all ICA-AROMA related components.
However, non-steady-state volume and motion estmation mertic RMSD \cite{jenkinson:2002} are still missing.
We consider to preprocess the nilearn demo dataset (\href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228}) with fMRIprep LTS release for getting all possible confounds for the future.
(Discussions amongs Pierre Bellec, Hao-Ting Wang, Elizabeth DuPre, and Chris Markiewicz)

## All contributor bot

\code{allcontributors} bot is added to track community contributions (Contributed by Pierre Bellec).

## Add \code{load\_confounds} to \code{nixtract}

In addition to the main package, there was a collaborative project with the developers of \href{https://github.com/danjgale/nixtract}{\code{nixtract}}.
\code{nixtract} is a tool that extract and process timeseries data from neuroimaging files.
Annabelle Harvey and Dan Gale added \code{load\_confounds} as a dependency of \code{nixtract} for reading fMRIPrep confound variables.

# Results

\code{load\_confounds} has now covered most of the noise components used in Ciric et al. \cite{ciric:2017}.
The following noise components and a set of paramaters for dedicated approaches are supported.
\begin{itemize}
  \item \code{motion}: the motion parameters including 6 translation/rotation (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{high\_pass}: basis of discrete cosines covering slow time drift frequency band.
  \item \code{wm_csf} the average signal of white matter and cerebrospinal fluid masks (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{global}: the global signal (\code{basic}), and optionally derivatives, squares, and squared derivatives (\code{full}).
  \item \code{compcor} \cite{compcor:2007}: the results of a PCA applied on a mask based on either anatomy (\code{anat}), temporal variance (\code{temp}), or both (\code{combined}).
  \item \code{ica\_aroma} \cite{icaaroma:2015}: the results of an idependent component analysis (ICA) followed by identification of noise components. This can be implementing by incorporating ICA regressors (\code{basic}) or directly loading a denoised file generated by fMRIprep (\code{full}).
  \item \code{scrub} \cite{power:2012}: regressors coding for time frames with excessive motion, using threshold on frame displacement and standardized DVARS (\code{basic}) and suppressing short time windows using the (Power et al., 2014) appreach (\code{full}).
\end{itemize}

\code{load\_confounds} can produce the following strategies from Ciric et al. \cite{ciric:2017}. The following table highlights the relevant options:

\begin{tabular}{ | l | c | c | c |  c |  c |  c |  c |  }
  \hline
  Strategy             & \code{high\_pass} & \code{motion} & \code{wm\_csf} & \code{global} & \code{compcor} & \code{ica\_aroma} & \code{scrub} \\
  \hline
  \code{Params2}       & x                &               & \code{basic}  &               &                &                  &              \\
  \code{Params6}       & x                & \code{basic}  &               &               &                &                  &              \\
  \code{Params9}       & x                & \code{basic}  & \code{basic}  & \code{basic}  &                &                  & \code{full}  \\
  \code{Params9scrub}  & x                & \code{basic}  & \code{basic}  &               &                &                  &              \\
  \code{Params24}      & x                & \code{full}   &               &               &                &                  &              \\
  \code{Params36}      & x                & \code{full}   & \code{full}   & \code{full}   &                &                  & \code{full}  \\
  \code{Params36scrub} & x                & \code{full}   & \code{full}   &               &                &                  &              \\
  \code{AnatCompCor}   & x                & \code{full}   &               &               & \code{anat}    &                  &              \\
  \code{TempCompCor}   & x                &               &               &               & \code{temp}    &                  &              \\
  \code{ICAAROMA}      & x                &               & \code{basic}  &               &                & \code{full}      &              \\
  \code{AROMAGSR}      & x                &               & \code{basic}  & \code{basic}  &                & \code{full}      &              \\
  \code{AggrICAAROMA}  & x                &               & \code{basic}  & \code{basic}  &                & \code{basic}     &              \\
  \hline
\end{tabular}

\smallskip
The future direction is to integrate \code{load\_confounds} as part of \code{nilearn} for better reach to wider range of users that can be benifited from the package.
To facilitate the nilearn inegration, we will add \code{sample\_mask} to support volume-sensoring based scrubbing and improve the \code{nilearn} \code{NiftiMasker} related feature.
