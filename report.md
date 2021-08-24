---
# Please do not delete --- above :)

# ========================================================================
# Automatically edited by GitHub actions, please do not modify     | START
# ==============================================================
report_url: https://github.com/brainhack-proceedings/template

last_changed: 2020-12-01 08:01 UTC

auth_created: r03ert0
# ===============================================================  | END

# Name of the event.
# -----------------
event: 'Brainhack Montreal 2020'

# The title of your report.
# -------------------------
title:  '`load_confounds`: fMRIprep confound regressor selection tool'

# Please SET this url to your GitHub project repo!
# -------------------------------------------------
url: https://github.com/SIMEXP/load_confounds

# Please edit affiliation details. The affiliation
# ids defined here will be used in the author list.
# -------------------------------------------------
affiliations:
- id: aff1
  orgname: 'Neuroscience, Brighton and Sussex Medical School'
  street: 'Trafford Centre'
  postcode: 'BN1 9RX'
  city: 'Brighton'
  state: 'East Sussex'
  country: 'United Kingdom'
- id: aff2
  orgname: 'Centre de recherche de l'Institut universitaire de gériatrie de Montréal'
  street: '4565 Queen Mary Road'
  postcode: 'H3W 1W6'
  city: Montreal
  state: Quebec
  country: Canada
# Please list the authors.
# If an author has multiple affiliations, please
# add corref field and indicate the primary affiliation.
# -----------------------------------------------------
author:
- initials: HTW
  surname: Wang
  firstname: Hao-Ting
  email: wang.hao-ting@criugm.qc.ca
  affiliation: aff1, aff2
  corref: aff2
  # Please make sure that you set corref (corresponding aff) if you have
  # multiple afiliations
- initials: JJD
  surname: Doe
  firstname: John J.
  email: johndoe@gmail.com
  affiliation: aff2
  url: https://jonhdoe.website.com

# Please write a brief summary of your project.
# This abstract will (only) appear on the webpage.
# -------------------------------------------------
summary: `load_confounds` is a tool for loading a sensible subset of the fMRI confounds generated with fMRIprep in python (Esteban et al., 2018). The outputs can be directly passes to Nilearn NifitMasker for denoising. The aim at Brainhack MTL 2020 is to implement new strategies as well as imporving the existing functions and documentations for a potential Beta release.

# Please add 1 to 3 tags
tags:
  - confounds
  - fMRIprep
  - nilearn

# Please comment out the following 5 lines if you have no supplemental material.
# supplemental:
#   - name: Material 1
#     url: https://osf.io
#   - name: Material 2
#     url: https://zenodo.org

coi: None.

acknow: The authors would like to thank the organizers and attendees of Brainhack Global 2020 and Brainhack Montreal 2020.

contrib: HTW [initials here] wrote the software, [initlas here] add new documentations, and HTW [initlas here] wrote the report.

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

<!-- The bibliography \code{report.bib} must respect \href{http://www.bibtex.org/Using/}{BibTeX} format.
You can cite entries in your bibliography using their tags:

\begin{itemize}
  \item Cite an article: \cite{author:2010}
  \item Cite a GitHub repository: \cite{githubrepo:2020}
\end{itemize}

\smallskip
\noindent You can use \code{inline code highlight}. This paragraph shows how to add blank lines and how to start a paragraph without indentation.

Remember that this is a LaTex flavored markdown. Therefore, some characters must be used with an escape character within the text:

\code{\& \% \$ \# \_ \{  \} \textbackslash} -->
fMRIprep (REF) is a popular minimal preprocessing software for functional MRI data.
‘Minimal preprocessing’ refers to motion correction, field unwarping, normalization, bias field correction, and brain extraction.
Confound regression and smoothing are exculded from the workflow.
Instead, fMRIprep provides users with a large set of potential confound regressors that covers many denoising strategies.
The users will have to select the confound regressors for denoising in the subsequent analysis.
Loading a sensible subset of confounds is difficult and error prone for many strategies, such as ICA-AROMA and CompCor.
\code{load\_confounds} can access confound variables and provides preset strategies for confound selections.
The loaded format is competible with \code{nilearn} analysis functions such as \code{NiftiMasker} and the GLM modules.
The aim is to provide a easy and foolproof API for users to perform subsequent denoising of \code{fMRIprep} output.

# Progress

At Brianhack Global Montreal 2020, the aim is to prepare the package ready for a potential Beta release.
The related issues involves completing the strategies missing and improve the user experience with better examples and error messages.
Several issues has been identified before Brainhack and the full discussion can be found under \href{https://github.com/SIMEXP/load\_confounds/issues\?q\=is\%3Aissue\+label\%3AbrainhackMTL2020\+i\s%3Aclosed}{\code{load\_confounds} GitHub issue}.

During Brainhack the following issues have been discussed and/or resolved:

## Strategies

We worked on three strategies:
\begin{itemize}
  \item ICA-AROMA (contributed by Hao-Ting Wang)
  \item Scrubbing (contributed by Steven Meisler)
  \item Anatomical CompCor (contributed by Steven Meisler)
\end{itemize}

## Demo

An executable demo using the nilearn developmental fMRI dataset (\href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228}) was added. (contributed by Michael W. Weiss)

## Error message

Exception raised when failing to find params in the confounds. (Contributed by François Paugam)

## Identify test dataset

\href{https://openneuro.org/datasets/ds003}{OpenNeuro ds003} is now the new test data. (Discussions amongs Pierre Bellec, Hao-Ting Wang, Elizabeth DuPre, and Chris Markiewicz)
Pierre Bellec plans to preporecess \href{https://openneuro.org/datasets/ds000228}{OpenNeuro ds000228} with all possible confounds.

## All contributor bot

\code{allcontributors} bot is added to track community contributions (Contributed by Pierre Bellec).

## Add \code{load\_confounds} to \code{nixtract}

In addition to the main package, there was a collaborative project with the developers of \href{https://github.com/danjgale/nixtract}{\code{nixtract}}.
\code{nixtract} is a tool that extract and process timeseries data from neuroimaging files.
Annabelle Harvey and Dan Gale added \code{load\_confounds} as a dependency of \code{nixtract} for reading fMRIPrep confound variables.

# Results

\code{load\_confounds} can now the following strategies from Ciric et al. \cite{ciric:2017}. The following table highlights the relevant options:

\begin{center}
\begin{tabular}{ | m{5em} | c | c | c |  c |  c |  c |  c |  }
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
\end{center}

The future direction is to integrate \code{load\_confounds} as part of \code{nilearn} for better reach to wider range of users that can be benifited from the package.
To facilitate the nilearn inegration, we will add \code{sample\_mask} to support volume-sensoring based scrubbing and improve the \code{nilearn} \code{NiftiMasker} related feature.


<!-- Subsection content goes here. You can create numerated lists:

\begin{enumerate}
  \item The labels consists of sequential numbers.
  \item The numbers starts at 1 with every call to the enumerate environment.
\end{enumerate} -->
<!--
### Equations & formulas
You can add mathematical formulas. Single dollars ($) are required for inline mathematics e.g. $f(x) = e^{\pi/x}$.
\smallskip

\noindent You can also use plain LaTeX for equations. These equations are rendered by MathJax, you can right click on them and explore the rendering options available at your browser!

\begin{equation} \label{eq:1}
\hat f(\omega) = \int_{-\infty}^{\infty} f(x) e^{i\omega x} dx
\end{equation}

and refer to \ref{eq:1} from text.

### Hypothes.is
We enabled \href{https://web.hypothes.is/}{hypothes.is} for the brainhack proceeding reports. This way, you can annotate, highlight and tag the content collaboratively! You may choose to share your insights with everyone, or keep them private. -->
<!--
# Results
Figure files must be placed at the \code{figures} folder. You can include figures using the following block:

\begin{figure}[h!]

  \includegraphics[width=.47\textwidth]{brainhack.png}

  \caption{\label{Figure-1} Your caption goes here.}

\end{figure}

Note that \code{width=.47 \textbackslash textwidth} above sets scales the figure size in the PDF. To change attributes of the figures on the webpage, please see \code{/figures/figures.css}.

To refer a figure in the text, you need to use the respective label defined in its caption: Fig. \ref{Figure-1} -->
