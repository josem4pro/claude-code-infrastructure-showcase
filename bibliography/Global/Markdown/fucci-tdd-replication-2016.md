# An External Replication on the Effects of Test-driven Development
## Using a Multi-site Blind Analysis Approach

**Authors**: Fucci, Davide et al.  
**Year**: 2016  
**Source**: ArXiv  
**ArXiv ID**: 1611.05994  
**URL**: https://arxiv.org/abs/1611.05994  
**Pages**: 14  
**Original File**: fucci-tdd-replication-2016.pdf

---

                                                                                                                                                                                            1




                                          A Dissection of the Test-Driven Development
                                         Process: Does It Really Matter to Test-First or to
                                                            Test-Last?
                                             Davide Fucci, Member, IEEE, Hakan Erdogmus, Member, IEEE Burak Turhan, Member, IEEE, Markku
                                                                     Oivo, Member, IEEE Natalia Juristo, Member, IEEE

                                               Abstract—Background: Test-driven development (TDD) is a technique that repeats short coding cycles interleaved with testing. The
                                               developer first writes a unit test for the desired functionality, followed by the necessary production code, and refactors the code. Many
arXiv:1611.05994v1 [cs.SE] 18 Nov 2016




                                               empirical studies neglect unique process characteristics related to TDD iterative nature. Aim: We formulate four process characteristic:
                                               sequencing, granularity, uniformity, and refactoring effort. We investigate how these characteristics impact quality and productivity in
                                               TDD and related variations. Method: We analyzed 82 data points collected from 39 professionals, each capturing the process used
                                               while performing a specific development task. We built regression models to assess the impact of process characteristics on quality and
                                               productivity. Quality was measured by functional correctness. Result: Quality and productivity improvements were primarily positively
                                               associated with the granularity and uniformity. Sequencing, the order in which test and production code are written, had no important
                                               influence. Refactoring effort was negatively associated with both outcomes. We explain the unexpected negative correlation with quality
                                               by possible prevalence of mixed refactoring. Conclusion: The claimed benefits of TDD may not be due to its distinctive test-first dynamic,
                                               but rather due to the fact that TDD-like processes encourage fine-grained, steady steps that improve focus and flow.

                                               Index Terms—Test-driven development, empirical investigation, process dimensions, external quality, productivity.
                                                                                                                          F



                                         1     I NTRODUCTION                                                                  scribed order of the main activities involved: writing a
                                                                                                                              failing test, making the test pass by adding production
                                         Test-driven Development (TDD) is a cyclic develop-
                                                                                                                              code, and refactoring [2]. Note that we distinguish be-
                                         ment technique. Ideally, each cycle coincides with the
                                                                                                                              tween test-first and TDD, with test-first referring to just
                                         implementation of a tiny feature. Each cycle finishes
                                                                                                                              one central aspect of TDD related to the sequencing of
                                         once the unit-tests spanning a feature, as well as all
                                                                                                                              the different activities involved in a typical cycle: writing
                                         the existing regression tests, pass. In TDD, each cycle
                                                                                                                              of test code precedes the writing of the production code
                                         starts by writing a unit test. This is followed by the
                                                                                                                              that the test code exercises.
                                         implementation necessary to make the test pass and
                                         concludes with the refactoring of the code in order to                                  Generalizing the above characteristics of a cycle in
                                         remove any duplication, replace transitional behavior,                               TDD (length, variation, and order) leads to three basic
                                         or improve the design. The cycles can be thought of as                               dimensions: we refer to them as granularity, uniformity,
                                         iterations of an underlying micro-process.                                           and sequencing. These dimensions do not apply just to
                                            Advocates of TDD believe that each such cycle, or                                 TDD but to any cyclic or iterative process. In particular,
                                         iteration, should have a short duration and that devel-                              it applies to several variants that differ from the text-
                                         opers should keep a steady rhythm [1], [2]. The common                               book version in one or more dimensions. Jeffries et
                                         recommendation is to keep the duration of the cycles to                              al. [1] see idealized TDD as the endpoint of a step-
                                         five minutes, with a maximum of ten [3], [4]. Thus TDD                               wise progression from a traditional, monolithic approach
                                         cycles are usually fairly short: we can call them micro-                             towards a modern, iterative one along these dimensions
                                         cycles or micro-iterations to distinguish them from longer                           . This progression creates a continuum of processes and
                                         notions of cycle or iteration that underlie lifecycle-scale                          gives rise to several viable variants of TDD, such as
                                         processes, such as sprints in Scrum [5].                                             incremental test-last (ITL) development [6]. 1
                                            TDD experts also recommend sticking with the pre-                                    Granularity refers to the cycles length, whereas unifor-
                                                                                                                              mity to their variation, i.e., how constant the duration of
                                                                                                                              the cycles is over time. Granularity and uniformity can
                                         • D. Fucci, B. Turhan, M. Oivo, and N. Juristo are with the Department of
                                           Information Processing Science, University of Oulu, Finland                        be thought of as the tempo and beat in music theory,
                                           E-mail: name.surname@oulu.fi                                                       respectively.
                                         • N. Juristo is also with Escuela Tecnica Superior de Ingenieros Informaticos,          A peculiarity of TDD is in the seemingly counterin-
                                           Universidad Politecnica de Madrid, Spain
                                           E-mail: natalia@fi.upm.es                                                          tuitive sequencing of the activities associated with each
                                         • H. Erdogmus is with Carnegie Mellon University, Silicon Valley Campus,
                                           USA.
                                                                                                                                1. Although TDD can be as well considered an emerging-design
                                           E-mail: hakan.erdogmus@sv.cmu.edu
                                                                                                                              practice, we focus on it as a development technique.
                                                                                                                             2



cycle: unit tests are written before production code. This        low level data (Section 4.4) about the development pro-
particular aspect constitutes the test-first component of         cesses used by our industrial partners. During the study,
TDD [2]. It is captured by the sequencing dimension.              in which a test-driven software development process
This aspect is counterintuitive because the normal soft-          was compared to an iterative test-last (ITL) process,
ware development workflow takes a test-last approach:             professional software developers tackled programming
traditionally, unit testing follows the implementation.           tasks of different nature (Section 4.5).
It is precisely flipping this component from a test-first            We visualized the micro-process underlying the data
to test-last dynamic that gives rise to the ITL variant.          collected from our industrial study on a participant-by-
Sequencing expresses the preponderance (or lack) of test-         participant basis, as shown in Figure 1. We were looking
first cycles in the development process.                          for a metric that would help summarize the process data
   A TDD cycle typically includes an additional activity,         for each participant in terms of the four dimensions. First
refactoring [2], in which the structure of the code is im-        we captured each dimension by a quantitative measure
proved internally without changing its external behavior          easily computable from the data. Then we attempted to
detectable by the existing tests [7]. Refactoring may             combine them into a single TDD compliance metric with
involve removal of duplication, other design improve-             an eye to regress this metric against the main outcome
ments, or replacement of temporary, stub code. At the             measures of external quality and developer productivity
end of each refactoring activity, all existing tests should       to assess its potential influence (external quality was
pass, showing that the intended behavior has not been             equated with functional correctness and developer pro-
affected by the design changes. However, in practice              ductivity with speed of production). To do this, three
refactoring is often inter-mixed with other activities, and       experts independently evaluated each participant’s data
as such, it sometimes involves adding new production              and gave each resulting observation a TDD compliance
code as well as changing existing tests and behavior.             score. We then tried to retrofit a formula that would
These practical deviations, however, also potentially nul-        mirror the experts’ overlapping assessments. However,
lify or reverse the hypothesized benefits [8], [9].               not all of the dimensions appeared to have an equally
   In summary, the process underlying TDD can be                  significant contribution. Additionally, some dimensions
characterized using three main dimensions: granularity,           appeared to have a close association with each other.
uniformity, and sequencing. The first two dimensions,             This revelation led us to decide to investigate the four di-
granularity and uniformity, deal with the iterative, or           mensions individually without attempting to aggregate
cyclic, nature of this process. The third dimension, se-          them into an ultimate compliance metric.
quencing, deals with the particular activities and their             To accomplish our goal, we decided to use data from
ordering within a cycle. The test-first nature of TDD             a set of ongoing quasi-experiments (i.e., studies that lack
is related to this third dimension. In addition, a fourth         a control experimental group, and where randomization
dimension that focuses on refactoring alone captures the          could not be applied) comparing TDD with ITL. The
prevalence of this activity in the overall process.               underlying data of this enclosing context had sufficient
   The goal of our paper is to understand, based on               variation to allow us to investigate the dimensions of
the four dimensions, how the elementary constituents of           interest using a continuum of micro-processes rather
TDD and related variants affect external software quality         than using the usual dichotomy of clearly delineated
and developer productivity. We answer the question:               treatment and control groups. Thus, this paper is the
which subset of these dimensions is most salient? This            result of an overlay observational study on top of an
information can liberate developers and organizations             experimental context. We call it an observational study [16]
who are interested in adopting TDD, and trainers who              because by pooling data from all experimental groups,
teach it, from process dogma based on pure intuition,             we effectively abstract away from them. In fact, we have
allowing them to focus on aspects that matter most in             no way of directly controlling the dimensions that serve
terms of bottom line.                                             as factors. The dimensions emerge due to interactions
   This paper is organized as follows: Section 2 presents         among several factors. A subject’s assignment to a par-
the rationale that motivates this study. The settings in          ticular experimental group with a prescribed process
which the study took place are described in Section 3,            is only one of these factors. Others possibly include
and Section 4 presents the study’s design. The results are        skill, ability, and motivation. Some factors are unknown.
presented in Section 5, the limitations are summarized            The dimensions are not binary in that instead of being
in Section 6, and related work is reported in Section 7.          present or absent, they are present to varying extents
The results are discussed in Section 8.                           independent of an assigned treatment. This implies that
                                                                  the dimensions lend themselves best to a regression-type
                                                                  analysis rather than hypothesis testing.
2   BACKGROUND AND M OTIVATION                                       Figure 2 illustrates how the data gathered from
What makes TDD “tick” has been a topic of research                subjects in a repeated-treatment design are used in
interest for some time [1], [10], [11], [12], [13], [14], [15].   this study versus in the larger context of the quasi-
In 2014, we set out to investigate this topic rigorously in       experiments. The observations are shared between the
the context of a larger research project. We had collected        studies. The outcome variables in both studies are exter-
                                                                                                                      3




Fig. 1: Example visualization of the micro-process underlying the development activities of subjects from a single
workshop run. The data were collected from Run 1 subjects in Figure 2 applying TDD or ITL to a set of programming
tasks. Each row corresponds to a task-subject pair, the subject applying either TDD or ITL to the task (task-technique
assignments are not specified in the figure).


nal quality and developer productivity. The sole factor      Finland, with development centres in Europe, Asia, and
in the experimental context is the treatment group: ITL      the Americas. Company B is an SME based in Estonia
or TDD. In the current study, all the observations are       that offers on-line entertaining platforms to businesses
pooled, and four measures are extracted from each ob-        worldwide. It has development centres in Europe, Asia,
servation to give rise to four factors, each corresponding   and the Middle-East.
to one process dimension.                                       This study analyzes aggregate data collected from
  Ordinarily, observational studies reveal associations:     four runs of a workshop about unit testing and TDD
their power to reveal causality is limited. However in       that were conducted at these two companies. In the
our case, there is a difference. The study is overlaid       unit testing portion of the workshop, we taught the
on top of an experimental context: the factors naturally     participants the principles of unit testing and how to
occur in the presence of a deliberate attempt to apply       apply unit testing using an iterative process, but based
a prescribed process. Similarly the outcome measures,        on a conventional test-last dynamic. In the TDD portion
the dependent variables, are observable as a result of the   of the workshop, we introduced how unit testing is
experimental context. Variable omission, or unaccounted      used to drive the development based on a TDD-style
confounding factors, is still a concern, but not more than   test-first dynamic. The workshop runs were held at the
they normally are in an experimental context.                participating companies’ sites. Three of runs were held
                                                             at three different sites of Company A (two in Europe
                                                             and one in Asia), and one run was held in Company B
3   S ETTING                                                 at a European site. Each run lasted five days (eight hours
The context of this study is a larger research effort        per day, including breaks) in which interactive lectures
investigating software engineering practices, including      were interleaved with hands-on practice sessions. Some
test-driven development, in industry [17]. This research     of the practice sessions focused on the use of iterative
initiative involves several industrial partners from the     unit testing with a test-last dynamic, which we refer to
Nordic countries. Two companies, referred to as Com-         as incremental test-last (ITL). The other sessions focused
pany A and Company B in the paper, participated in this      on TDD. Each participant took part in a single run of the
study. Company A is a large publicly-traded provider of      workshop and implemented several tasks, designed for
consumer and business security solutions. It is based in     the purpose of the workshop using the techniques that
                                                                                                                      4




Fig. 2: The observations gathered from participants were divided according to the particular technique used by
distinct experimental groups formed for the original quasi-experiment (not reported in this paper). In the quasi-
experiment, the data were collected from iterative test last (ITL) and test-driven development (TDD) sessions during
four runs. Note that Run 4 had a different structure than the other three in terms of the order and development
method of the tasks used. For the purpose of the study reported in this paper, however, all the observations from
all runs and groups were pooled together, as illustrated under the column labeled ”This Study”.


the participant was taught. We used five tasks to support   4.1   Research Questions
the teaching in a Randori fashion; the remaining three
                                                            We focus on the following research questions with re-
tasks (see Section 4.5) were implemented in solo mode,
                                                            gard to two outcomes— external software quality and
and represent the experimental objects of this study. We
                                                            developer productivity—and four factors corresponding
also encouraged the participants to try to apply the
                                                            to the four process dimensions—sequencing, uniformity,
techniques that they learned during the workshop in
                                                            granularity, and refactoring effort.
their regular work, although we did not collect any data
from these informal practice attempts. The workshop           • RQ-QLTY: Which subset of the factors best explain
runs were part of a bigger, ongoing multi-object quasi-         the variability in external quality?
experiment on TDD with a repeated-treatment design.           • RQ-PROD: Which subset of the factors best explain
                                                                the variability in developer productivity?
4   S TUDY D ESIGN                                             The notion of external quality in RQ-QLTY is based
In this section, we state the research questions that are   on functional correctness, specifically average percentage
answered in the study. We also elaborate on the choice      correctness. We do not consider other notions of external
of the research method, the constructs of interest, the     quality, such as usefulness and usability. The notion of
metrics used to measure them, the experimental tasks        productivity in RQ-PROD is based on speed of produc-
used, and the data analysis techniques.                     tion, or amount of functionality delivered per unit effort.
                                                                                                                           5



  We also consider how the factors interact with each          tests. The formula to calculate QLT Y is given in Equa-
other (for plausible interactions) and whether these in-       tion 1,
teractions matter in predicting the levels of the outcomes.                           #TPU S
                                                                                             QLT Yi
                                                                             QLT Y = i=1            × 100,         (1)
                                                                                         #T U S
4.2     Method
                                                               where QLT Yi is the quality of the i-th tackled user-story,
We collected fine-grained process data from the four           and is defined as in Equation 2.
workshop runs to quantify the outcome (dependent)
variables and factors (independent variables). For each                                  #ASSERTi (P ASS)
                                                                            QLT Yi =                      .              (2)
outcome variable, we use regression analysis to explore                                  #ASSERTi (ALL)
its relationship to the four factors and possible interac-
tions among the factors. We also aim to understand the         In turn, the number of tackled user stories (#T U S) is
form of any underlying relationships. To have enough           defined, similarly to Erdogmus et al. [11], as in Equation
variability among the process dimensions represented           3.                   (
                                                                                 n
by the factors, we used pooled data from the four runs,                         X     1 #ASSERTi (P ASS) > 0
                                                                     #T U S =                                          (3)
which mixed the application of TDD and ITL.                                     i=0
                                                                                      0 otherwise
   In our specific case, a controlled experiment was in-
feasible since it is not possible to control the factors. In   where n is the number of user-stories composing the
this regard, this study is neither a controlled experiment     task. #ASSERTi (P ASS) is the number of passing JUnit
nor a quasi-experiment. No experimental groups (control        assert statements2 in the acceptance test suite associ-
and treatment groups) can be created by manipulating           ated with the ith user-story. Accordingly, a user-story is
the factors. A priori random assignment of subjects to         considered as tackled if at least one of its JUnit assert
factor groups is not possible without such manipulation.       statements passes. In particular, an empty implementa-
                                                               tion will not pass any assert statements; conversely, a
                                                               hard-coded or stubbed implementation may pass one or
4.3     Outcome (Dependent) Variables                          more assert statements. In the latter case, a user story
                                                               is still considered as tackled since there is an indication
The outcomes under study were external quality, repre-         that some work on it has been done. We use an assert
sented by a defect-based (or dually, correctness-based)        statement as our smallest unit of compliance with the
metric (QLT Y ), and productivity, represented by an           requirements. This allows us to be able to better high-
effort-based metric (P ROD). To be able to measure levels      light quality differences between observations, relative to
of these metrics, we divided the programming tasks             the enclosing acceptance test case. The QLT Y measure
used in the workshop runs into many smaller sub-               deliberately excludes unattempted tasks and tasks with
tasks. Each sub-task resembled a small user story. A user      zero success; therefore, it represents a local measure
story is a description of a feature to be implemented          of external quality: it is calculated over the subset of
from the perspective of the end user, and we adopted           user stories that the subject attempted. QLT Y is a ratio
this perspective when defining the sub-tasks. Because          measure in the range [0, 100].
the sub-tasks were very granular, they captured the               For example, suppose that a subject tackles three user
tasks’ functional requirements with high precision. The        stories; i.e., #T U S = 3. This means there are three user
high level of granularity also made it possible to assess      stories for which at least one assert statement passes in
task completion, quality, and correctness on a numerical       the associated acceptance test suite. Say, the acceptance
scale rather than a binary scale. The user stories were        tests of the first tackled user story contains 15 assertions,
frozen and known to the subjects upfront: they did not         out of which five are passing; the seconds contains ten
change during the implementation of the tasks by the           assertion, and two are passing; the third also contains
subjects. An acceptance test suite—a set of reference          ten assertions, and eight are passing. Then the quality
tests developed by the researchers—was associated with         value of the first tackled user story is 0.33; the second
each user story, with disjoint subsets of acceptance tests     user story has a quality value of 0.20, and the third 0.80.
in each suite covering specific user stories (see Section      Therefore, the QLT Y measure for the subject is 44%, the
4.5 for details). The acceptance tests were then used to       average of the three attempted sub-tasks (100 × (0.33 +
calculate the two outcome metrics, as explained below.         0.20 + 0.80)/3).
Both metrics, with minor variations, have previously
been used in TDD research [11], [18], [19], [20].
                                                               4.3.2   Productivity
                                                               The metric for productivity (P ROD) follows the soft-
4.3.1    External Quality
                                                               ware engineering definition of “work accomplished, with
The metric for external quality QLT Y measures how             the required quality, in the specified time” [21]. The metric
well the system matches the functional requirements.
Each user story is expressed as a subset of acceptance          2. http://junit.org/apidocs/org/junit/Assert.html
                                                                                                                              6



is calculated as in Equation 4 and is defined as a ratio       Each colored vertical section represents a cycle. The
measure in the range is [0, 100].                              color encodes the type of a cycle. The width of the
                              OU T P U T                       section represents the cycle’s duration. Figure 3 captures
                  P ROD =                                (4)   data collected from a single subject during one of the
                                 T IM E
                                                               runs presented in Figure 2. After a first test-first cycle
OU T P U T (Equation 5) is the percentage of assert state-     (test-first production, light blue), the subject wrote tests
ments passing for a task when the subject’s solution is        (test addition, aqua) without adding new functionality.
ran against the whole acceptance test suite for that task.     New production code was implemented using a test-last
It conveys the proportion of the system requirements           approach (test-last production, violet), and after that the
that have been correctly implemented. Unlike QLT Y ,           subject switched to adding new production code using
this metric is independent of how many of the task’s           a test-first dynamic (test-first production again, light
user stories the subject tackled. It does not apply the        blue). The majority of the final cycles were dedicated
”at-least-one-assertion” filter to sub-tasks to guess which    to refactoring (orange).
sub-tasks were attempted and select only those tasks.             Having characterized the components of the develop-
                       #ASSERT (P ASS)                         ment process in terms of cycle types and durations, we
        OU T P U T =                   × 100,            (5)   can now relate these components to the four process
                       #ASSERT (ALL)
                                                               dimensions to determine their levels for a subject-task
  T IM E (Equation 6) is an estimate of the number of          pair. Granularity (GRA) is measured by the median of
minutes a subject worked on a task. It was based on log        cycle duration. Uniformity (U N I) is measured by the
data collected from the subject’s IDE.                         median absolute deviation (MAD) of cycle duration.
                             tclose − topen                    We selected median and MAD, respectively, over other
                 T IM E =                               (6)
                                  6000                         typical value and dispersion measures because they are
where t denotes a timestamp in millisecond. Thus               more robust with respect to outliers. Sequencing (SEQ)
T IM E is simply the difference, in minutes, between           is measured by the fraction of test-first cycles, that is, test-
the timestamp of closing the IDE and the timestamp             first production cycles, which start with a test addition
of opening the IDE. If the subject opended and closed          activity, followed by the addition of production code
the IDE several times during a run, T IM E was cal-            in the middle, and end with successful passing of all
culated as the sum of the all open-close intervals. The        tests. Finally, refactoring effort (REF ) is measured by the
maximum time allocated to a task was ∼300 minutes,             fraction of refactoring cycles in the development process.
and one minute was the smallest time unit considered.          In refactoring cycles, normally production or test-code is
For example, suppose a subjects implements a task with         modified, but no new test or production code is added.
a total of 50 assert statements in its acceptance test         Table 1 summarizes these four process dimensions rep-
suite. After running the acceptance test suite against the     resenting the factors, or independent variables, whereas
subject’s solution, 45 assert statements are passing. Then     Table 2 summarizes the heuristics used by the IDE plug-
OU T P U T = 100 × 45/50 = 90%. Suppose the subject            in to identify the cycle type according to the underlying
delivered the solution in three hours (i.e., T IM E = 180      sequence of actions. In the following section, we explain
minutes). The subject’s P ROD is therefore 0.50, denoting      the computation of the level of each process dimension
an assertion passing rate of .50% per minute.                  and the implications of the chosen metrics.


4.4   Factors (Independent Variables)
In order to quantify the levels of the four factors—
granularity, uniformity, sequencing, and refactoring
effort—we decompose the development process applied
by a subject into a sequence of small cycles. A cycle
is delimited by the successful execution of a regression
test suite (the green bar in JUnit). Each cycle in turn is
composed of a sequence of more elementary actions. The
                                                               Fig. 3: Example visualization of a sequence of devel-
actions form a pattern which help identify a cycle’s un-
                                                               opment cycles for a single subject working a particular
derlying type of activity—for example, refactoring, test-
                                                               task, equivalent to a single row of Figure 1. The width
first production, test-last production, or test addition.
                                                               of each section represents the cycle’s duration and the
This type is inferred automatically by a tool [22] installed
                                                               color represents the type of the cycle.
in the IDE and using the heuristics devised in Kou et
al. [23]. A cycle also has a duration calculated as the
difference between the timestamps of the first and last
actions in the cycle.                                          4.4.1   Granularity (GRA)
   An example is shown in Figure 3. In the example,            Granularity is measured as the median cycle duration
time flows from left to right, measured in minutes.            in minutes. This measure captures the extent of the
                                                                                                                                                  7


                      TABLE 1: Summary of study factors corresponding to the four process dimensions.
              Dimension     Interpretation                                                                                         Range
              GRA           A fine-grained development process is characterized by a cycle duration typically between 5            [0, 300]
                            and 10 minutes. A small value indicates a granular process. A large value indicates a coarse
                            process.
              UNI           A uniform development process is characterized by cycles having approximately the same                 [0, 149]
                            duration. A value close to zero indicates a uniform process. A large value indicates a
                            heterogeneous, or unsteady, process.
              SEQ           Indicates the prevalence of test-first sequencing during the development process. A value close        [0, 100]
                            to 100 indicates the use of a predominantly test-first dynamic. A value close to zero indicates
                            a persistent violation of the test-first dynamic.
              REF           Indicates the prevalence of the refactoring activity in the development process. A value close         [0, 100]
                            to zero indicates nearly no detectable refactoring activity (negligible refactoring effort). A value
                            close to 100 indicates a process dominated by refactoring activity (high refactoring effort).

                    TABLE 2: Heuristics used by the tool to infer the type of development cycle (from [23]).
 Type                Definition
                     Test creation → Test compilation error → Code editing →Test failure → Code editing → Test pass
                     Test creation → Test compilation error → Code editing → Test pass
 Test-first
                     Test creation → Code editing → Test failure → Code editing → Test pass
                     Test creation → Code editing → Test pass
                     Test editing (file size changes ± 100 bytes) → Test pass
 Refactoring         Code editing (number of methods, or statements decrease) → Test pass
                     Test editing AND Code editing → Test pass
                     Test creation → Test pass
 Test addition
                     Test creation → Test failure → Test editing → Test pass
                     Code editing (number of methods unchanged, statements increase) → Test pass
 Production
                     Code editing (number of methods increase, statements increase) → Test pass
                     Code editing (size increases) → Test pass
                     Code editing → Test creation → Test editing → Test pass
 Test-last
                     Code editing → Test creation → Test editing → Test failure → Code editing → Test pass
 Unknown             None of the above → Test pass



cyclic nature of the development process used. The lower                      4.4.3 Sequencing (SEQ)
the value, the finer-grained in general the process is. A                     Sequencing characterizes the underlying development
subject with a low GRA value tends to use short cycles,                       process according to the extent to which a developer
and completes more cycles within the same time than                           adheres to a test-first dynamic. It is measured as the
a subject with a higher GRA value. We use the median                          percentage of cycles having type test-first. Note that
value rather than the average to reduce the sensitivity of                    SEQ does not capture the full nature of TDD, just one
the measure to outliers. GRA is a ratio measure in the                        central aspect that has to do with its dynamic.
range [0, 300] because the maximum time allocated for a
task was 300 minutes.                                                           SEQ (Equation 8) is a ratio measure in the range [0,
                                                                              100] and it is calculated as follows:
                                                                                                    (
4.4.2    Uniformity (UNI)                                                                     Pn      1 type(i) = test-first
                                                                                                i=0
                                                                                                      0 otherwise
Uniformity represents the dispersion of cycle duration,                             SEQ =                                    × 100, (8)
as measured by mean absolute deviation (MAD).                                                               n
  Uniformity indicates the extent to which a subject was                        where n is the total number of cycles.
able to keep the rythm of the development cycles. The
                                                                              4.4.4 Refactoring Effort (REF)
lower the value, the more uniform the cycles were. A
value of zero indicates that all the cycles had the same                      Refactoring effort (refactoring in short) is measured by
duration. UNI (Equation 7) is a ratio measure in the                          the percentage of cycles likely to be associated with
range [0, 149], and calculated as follows:                                    refactoring activity. A cycle recognized by the IDE tool
                                                                              to be an instance of successful refactoring activity has
                                                                              type ref actoring.
                                                                                REF (Equation 9) is a ratio measure in the range [0,
   U N I = median(| duration(i) − GRA |), i ∈ [0, n]                   (7)    100], and it is calculated as follows:

where n is the total number of cycles.                                                               (
                                                                                              Pn      1     type(i) = refactoring
  We use MAD as opposed to standard deviation be-                                                i=0
cause GRA is defined as a median. Again this helps                                                    0     otherwise
                                                                                   REF =                                               × 100,   (9)
reduce the influence of outliers, atypical cycles that are                                              n
too short or too long.                                                        where n is the total number of cycles.
                                                                                                                                     8



4.5   Study Tasks                                             and how they interact).
                                                                 The first requirement was algorithmic, and the re-
The participants implemented three tasks during each
                                                              maining requirements were more structural in nature.
workshop run. The first two tasks were greenfield: they
                                                              The associated acceptance test suite consisted of 132
involved implementing a solution from scratch. The
                                                              JUnit assert statements, covering the 11 sub-stories. We
third task was brownfield: it involved extending an
                                                              allocated five hours for this task because it was deemed
existing system. The third task was more difficult than
                                                              more difficult than Task 1 and Task 2.
the first two. We describe each task below.
                                                                 Table 3 shows how the participants were distributed
   Task 1 (Mars Rover) involved the implementation of
                                                              over the tasks during the four runs of the workshop.
the public API for moving a vehicle on a grid. It was sim-
                                                                 The total number of data points collected over the
ple and algorithmic in nature, but required several edge
                                                              three tasks and three workshop runs was 99. After
cases to be handled as well as correctly parsing the input.
                                                              removing the data points containing missing values, our
The task is commonly used in the agile community
                                                              final dataset consisted of 82 data points (27 for Task 1,
to practice coding techniques. The requirements were
                                                              25 for Task 2, and 30 for Task 3).
described as six user stories, which were later refined
to 11 sub-stories for acceptance testing. The participants
were given the six user stories, the stub for the class       4.6       Instrumentation
definition, including the API signature (8 LOC), and a
stub of the JUnit test class (9 LOC). The acceptance test     The development environment used by the participants
suite had 89 JUnit assert statements that covered the 11      included Java version 6, Eclipse 4, and JUnit 4. The IDE
sub-stories. We allocated four hours for this task.           was instrumented with the Besouro tool [22] to collect
   Task 2 (Bowling Scorekeeper) required the implemen-        data about the development cycles.
tation of a system to keep scores for a bowling game.            Besouro’s data were used to calculate the metrics
This task is also well known and has been used in             representing the factors. This tool is capable of recog-
previous TDD studies [24].                                    nizing development actions (e.g., creating a test class,
   Also algorithmic in nature, its specification included     running the unit tests, modifying a method) taking place
13 user stories to be implemented in a specific order.        inside the development environment and logging them
Each user story contained an example scenario. The            along with a timestamp. The information is then used
participants were given a stub containing two classes         to aggregate a sequence of development actions into
(23 and 28 LOC) with the signatures of the methods to         identifiable micro-cycles. The cycles are then classified
be implemented. A stubbed test class (9 LOC) was also         as test-first compliant, refactoring, or non-test-first com-
provided. The acceptance test suite of this task had 58       pliant (with three subcategories: test-last production, test
JUnit assert statements that covered the 13 user stories.     addition, regression test) according to an expert system
We also allocated four hours for this task.                   based on the heuristics presented in Kou et al. [23]. This
                                                              classification gives rise to the type of the cycle used in
   Task 3 (MusicPhone) was about extending a con-
                                                              defining the factors SEQ and REF . For example, the
cert recommendation system. The system has a tradi-
                                                              following sequence of development actions is classified
tional three-tier architecture that mimics a real-world
                                                              as a test-last production cycle since the production code
application (see the task documentation included in the
                                                              was written (and compiled) before the unit test:
replication package [25]). We provided the participants
with a partially working implementation consisting of
13 classes, and 4 interfaces (1033 LOC). The existing               •    EditAction 1397055646 Frame.java ADD int score() METHOD
implementation included the UI and data access tiers.                    bytes: 127
                                                                    •    CompilationAction 1397055689 BowlingGame OK
MusicPhone does not use any additional frameworks                   •    EditAction 1397055691 FrameTest.java ADD void testScore()
with the exception of JUnit for unit testing and Swing                   METHOD bytes: 391
for the user interface. The existing part of the task               •    UnitTestSessionAction 1397055770 FrameTest OK
was developed using the Singleton pattern, which was
explained to the subjects before the start of the exper-
iment. The subjects were also given a cheat sheet of             As another example, the following sequence is clas-
the architecture before they started working on the task.     sified as test-first compliant, or a test-first production
The participants were asked to implement three missing        cycle:
requirements in the business logic tier. They did not have
to modify the presentation tier. These requirements were
                                                                    •    EditAction 1397056134 BowlingGameTest.java ADD void
later refined into 11 sub-stories for acceptance testing.                testScore() METHOD bytes: 126
   In order to implement these requirements, the partic-            •    UnitTestSessionAction 1397056135 BowlingGameTest FAIL
                                                                    •    FileOpenedAction 1397056152 BowlingGame.java
ipants needed to understand the existing system and its             •    EditAction 1397056173 BowlingGame.java ADD int score()
architecture. We provided the participants with a smoke                  METHOD bytes: 340
test (38 LOC, 6 JUnit assertions) as an example of the              •    UnitTestSessionAction 1397056176 BowlingGameTest OK
usage of the existing classes (how to instantiate them
                                                                                                                                     9


             TABLE 3: Allocation of the participants to tasks and techniques (ITL indicates iterative test-last).
                                       Company                   Task 1               Task 2         Task 3
                                   Company A (3 runs)           18 (ITL)             19 (TDD)       21 (TDD)
                                   Company B (1 run)       6 (ITL)   9 (TDD)    2 (ITL)   8 (TDD)   16 (TDD)



   In this last example, a unit test for a not-yet-existing               the relationship between the outcome variables and the
functionality is written first. The test fails since there                complete set of factors. Finally, we use feature selec-
is no production code that can be exercised by it. The                    tion to create the model that includes only the most
developer then writes some production code after which                    salient factors. For each model we report the effect size
the test passes.                                                          as adjusted R2 , which indicates the proportion of the
   Each development cycle also has a duration derived                     variance in the outcome variable that the factors are able
from the timestamps (not shown here) of the delimiting                    to explain. Thus 1 − adjusted R2 can be attributed to
actions. The duration is used to calculate GRA and U N I.                 unknown factors, or inherent variability in the outcome
                                                                          variable. As opposed to the p-value, which informs
4.7   Subjects                                                            whether the association is present by chance or not, the
                                                                          effect size informs about the magnitude of the association.
The subjects were software professionals of varying
                                                                          The soundness of the models is checked by applying the
levels of experience. For the workshops organized at
                                                                          proper assumption diagnostics. The data analysis was
Company A’s sites, 24 developers signed up, but 22 par-
                                                                          carried out using R version 3.1.2. with α level set to
ticipated in total. In Company B, after 19 initial signups,
                                                                          .05. The dataset used in the analysis is included in the
17 participated in the workshop. All the subjects except
                                                                          replication package [25].
two had at least a bachelor’s degree in computer science
or a related field. The average professional experience
in Java development was 7.3 years (standard dev. = 4.1,                   5     R ESULTS
min. = 1, max. = 15 years). Before the workshop, the                      We collected six measures. The main four process di-
participants were asked to state their level of skill in                  mensions (GRA, U N I, SEQ, and REF ) constitute the
Java and unit testing on a 4-point Likert scale. Table 4                  factors (independent variables). QLT Y , and P ROD are
shows the breakdown of their answers.                                     the outcome (dependent) variables.
                                                                            The dataset includes 82 complete observations. An
TABLE 4: Distribution of the subjects over the four skill
                                                                          observation was considered complete when there were
levels for Java and unit testing (in percentage).
                                                                          no missing values for any of the variables over the three
                                        Unit testing                      tasks.
                            None   Novice   Intermediate   Expert
             None            2.5    9.5           2.5       2.5
             Novice          27     17            9.5        0
      Java




             Intermediate    2.5    9.5           2.5        5            5.1    Descriptive Statistics
             Expert           0     2.5           2.5        5
                                                                          The descriptive statistics for the dataset are reported in
                                                                          Table 5. The histogram and probability density of each
   Most of the subjects had basic experience with Java
                                                                          variable is shown in Figure 4.
and unit testing. Few subjects (5%) considered them-
                                                                             QLT Y appears normally distributed, with about 75%
selves experts in both Java and unit testing. Fewer
                                                                          of the observations in the 50-100 range, as shown in
(2.5%) said they had neither skill. No one declared to
                                                                          Figure 4e. P ROD appears positively skewed and multi-
be expert unit testers. To sum up, the subject sample
                                                                          modal as shown in Figure 4f. Approximately 25% of the
was composed mostly of mid-level Java developers with
                                                                          observations are in the 0-0.25 range (min = 0, max =
basic unit testing skills, but experience and skill still
                                                                          .86). The shapes of the distribution of QLT Y and P ROD
varied. In order to establish a better baseline and level
                                                                          are consistent with those of a previous experiment con-
the playing field, the subjects were given a five-hour
                                                                          ducted with student subjects [26].
hands-on tutorial on unit testing principles using JUnit
                                                                             The distributions of GRA (Figure 4a) and U N I (Figure
and a five-hour training on applying TDD in different
                                                                          4b), variables capturing the cyclical characteristics of the
contexts.
                                                                          process, are positively skewed. For both, the majority
                                                                          of the observations are in the 0-10 interval. The peek of
4.8   Data Analysis Techniques                                            the granularity distribution is close to the suggested five
Our analysis starts with the characterization of the data                 minutes; also the peek and skewness of the uniformity
with descriptive statistics. The next step is to look for cor-            distribution suggest that the majority of the subjects kept
relations among the variables. This allows us to evaluate                 a consistent rhythm (U N I = 0 minute indicates that all
the redundant information in the factors (independent                     the cycles had the same duration).
variables) and get a glimpse of the associations between                     Figure 4c shows that some subjects, around a quarter,
the outcome (dependent) variables and factors. Next                       wrote tests first only to some extent (approximately
we create multiple linear regression models to identify                   one-third of the total cycles completed). Subjects in the
                                                                                                                          10


                      TABLE 5: Descriptive statistics for the dataset used in the study (n=82).

                     Statistic    Mean    St. Dev.   Min     Pctl(25)   Median    Pctl(75)     Max
                     QLTY         65.55    19.25     0.00     50.00      66.67     81.67      100.00
                     PROD           .24      .26      .00       .05        .12       .35        .86
                     GRA           7.93     9.53      .87      2.78       4.36      8.95       48.97
                     UNI           4.52     9.14      .00      2.43       4.52      8.17       51.32
                     SEQ          32.79    21.86      .00     16.66      33.33     47.33       87.50
                     REF          24.39    18.50      .00      8.71      23.07     34.52       71.42



upper quantile wrote test-first approximately half of the       TABLE 7: Correlations between the factors and out-
time. The distribution of REF is positively skewed, as          comes. The p-values are reported in parentheses.
shown in Figure 4d. The subjects applied refactorings                                 GRA     UNI      SEQ     REF
approximately one-third of the time on average, but a                                 -.27    -.36     .12     -.11
                                                                             QLTY
                                                                                      (.02)   (.01)    (.29)   (.31)
quarter of them refactored less than 10% of the time.                                 -.20    -.25     .03     -.17
This behavior is not surprising as it often happens in                       PROD
                                                                                      (.07)   (.20)    (.81)   (.11)
real projects, as was observed by Aniche et al. [27].

5.2 Correlation Analysis                                        optimal models that explain how useful the factors are
In this section we check for significant correlations be-       in predicting the observed outcomes. We start with a
tween the variables, using the Spearman ρ coefficient           full model that includes all the factors and employ a
[28]. Spearman’s correlation is a rank-based correlation        backward elimination approach to find the model that
measure that does not rest upon an assumption of                minimizes information loss according to Akaike’s Infor-
normality, and it is robust with respect to outliers.           mation Criterion (AIC) [30]. Minimizing AIC is preferred
   Table 6 summarizes the correlations among the four           to maximizing adjusted R2 because it favors the simplest
factors. The factors dealing with the cyclical character-       model [30], whereas adjusted R2 can be biased towards
istics of the process, GRA and U N I, are positively cor-       the more complicated model. The initial model we
related (ρ = .49, p-value = .0001). The shorter the cycles      propose has two high-level components, with possible
tend to be, the more uniform they are, and vice versa.          interaction within the two high-level components, but
Another significant correlation exists between GRA and          no interactions across them.
REF , but the magnitude is small (ρ = −.22, p-value                • The high-level component CY CLE deals with the
= .02). Coarser processes appear to involve reduced                   cyclical characterisics of the development process.
refactoring effort, and vice versa.                                   This component includes the factors granularity and
   Table 7 summarizes the correlations among the depen-               uniformity, and the interaction between them.
dent (outcome) variables and independent variables (fac-           • The high-level component T Y P E deals with the
tors). QLT Y is negatively correlated with GRA, i.e., the             specific approach used within cycles. This compo-
smaller the cycle, the higher the quality, and vice versa.            nent includes the factors sequencing and refactor-
The correlation is significant (ρ = −.25, p-value=.02)                ing, and the interaction between them.
and its magnitude is medium [29]. A similar negative            It is reasonable to assume that CY CLE and T Y P E do
association exists between QLT Y and U N I; the more            not interact because they represent orthogonal higher-
uniform the development cycles, the better the external         order concepts.
quality (ρ = −.36, p-value = .01), and vice versa.                 Hence, our model is CY CLE + T Y P E, where:
   Unlike QLT Y , none of the factors correlate signifi-           • CY CLE = GRA + U N I + (GRA : U N I)
cantly with the outcome variable P ROD.                            • T Y P E = SEQ + REF + (SEQ : REF )
                                                                Here the symbol ”:” denotes interaction between two
TABLE 6: Correlations among the factors.
                                                                factors. Thus we have:
The values above the diagonal represent Spearman’s
correlation coefficient; the values below represent the p-          OU T COM E ∼ GRA + U N I + GRA : U N I +
                                                                                                                        (10)
value.                                                                                  SEQ + REF + SEQ : REF
                    GRA     UNI    SEQ    REF                   where OU T COM E ∈ [QLT Y , P ROD].
             GRA            .49    .11    -.22
             UNI    .0001          -.11   .01
                                                                   The results for this model are reported in Table 8. The
             SEQ    .15     .92           -.48                  model in Equation 10 is statistically significant for both
             REF    .02     .30    .10                          outcome variables according to F-statistic. The observed
                                                                effect sizes (measured by adjusted R2 ) are considered
                                                                small-medium [29], typical for studies involving human
5.3 Regression Models                                           factors [31], [32]. However, the factor coefficients and
In this section, we explore the relationships between           interaction terms are all statistically insignificant at the
the four factors and two outcome variables and find             given α-level. The correlations observed between the two
                                                                                                                                 11




                          (a) Granularity                                                     (b) Uniformity




                           (c) Sequencing                                                    (d) Refactoring




                              (e) Quality                                                   (f) Productivity
Fig. 4: Histograms and density curve for the study variables. The x-axis for the histograms of granularity and
uniformity are cut to maximum value of the dataset, instead of extending to the theoretical maximum.


factors GRA and U N I (see Section 5.2) may indicate a                    Based on these results, we drop the problematic inter-
problem of multicollinearity in the models. Although it                 action factor GRA : U N I from further analysis. There-
does not impact the power of the model, multicollinear-                 fore, we modify the candidate model for QLT Y to be:
ity3 has the risk of inaccurate regression coefficients and
p-values [33]. We can assess the extent of such overfitting              QLT Y ∼ GRA+U N I +SEQ+REF +SEQ : REF (11)
using the Variance Inflation Factor (VIF). VIF indicates to
                                                                        5.3.1 Feature Selection
what extent the variances of the coefficients estimates are
inflated due to multicollinearity. A VIF value exceeding                We now investigate whether the model in Equation 11
4.0 is interpreted as problematic multicollinearity [34].               can be further simplified by applying feature selection.
The interaction term GRA : U N I exhibits a VIF of 5.53,                Our approach, stepwise backward selection [35], iter-
which is excessive. For the factors and the other interac-              atively drops the least significant variable to form a
tion term, we have VIF(GRA) = 3.39, VIF(U N I) = 1.91,                  new model based on its AIC. The re-inclusion of any
VIF(SEQ) = 2.38, VIF(REF ) = 3.24, and VIF(SEQ :                        previously dropped variables is considered at the end of
REF ) = 2.56, which are acceptable.                                     each iteration.
                                                                           AIC represents the information loss for a given model
                                                                        relative to the theoretical ideal, unlike the Chi-squared
                                                                        goodness-of-fit approaches in which the absolute model
  3. Multicollinearity results from two or more factors in a model
being significantly correlated, resulting in redundancy, and possible   fit is assessed [36]. AIC takes into account the complexity
overfitting.                                                            of the model, i.e., the number of variables. As opposed to
                                                                                                                                   12


      TABLE 8: Regression summary for the models in Equation 10. Standard error reported in parentheses.

                                                                             Outcome variable:
                                                                   QLTY                    PROD
                                GRA                              −.36 (0.19)      −5.03e−03 (2.58e−02)
                                                                p-value = .65          p-value = .05*

                                UNI                               −.62 (.49)      −9.96e−03 (6.53e−03)
                                                                p-value = .20         p-value = .13

                                SEQ                               .002 (.14)      −1.48e−03 (1.80e−03)
                                                                p-value = .86         p-value = .41

                                REF                              −0.26 (.20)      −4.91e−03 (2.53e−03)
                                                                p-value = .17         p-value = .05

                                GRA:UNI                           .006 (.04)       1.57e−04 (1.87e−04)
                                                                p-value = .65          p-value = .40

                                SEQ:REF                          .001 (.005)       3.60e−05 (7.49e−05)
                                                                p-value = .80          p-value = .62

                                Constant                         77.27 (7.14)             0.47 (.09)
                                                                p-value = .000          p-value = .000
                                Adjusted R2 [conf. int.]         .09 [.01, .26]          .08 [.01, .26]
                                Residual Std. Error (df = 75)        18.40                     .24
                                F Statistic (df = 6; 75)              2.42                    2.72
                                p-value                                .03                     .04



R2 -based indicators [36], it does not inflate fitness after            TABLE 9: Regression summary for the QLT Y model
more variables have been added. Another relative fitness                having the best AIC (483.50). Standard error reported
index that takes into account model parsimony similar                   in parentheses.
to AIC is BIC [30]. BIC tends to penalize the number of
                                                                                                             Outcome variable:
variables in the model more than AIC. However, unlike
                                                                                                                  QLTY
BIC, AIC reaches an asymptotically optimal model under
                                                                                  GRA                          −.34 (0.17)
the assumption that the best theoretical model is not                                                         p-value = .04
among the possible candidates [37]. This property of AIC
makes it favorable to BIC in our case: the best model for                         UNI                           −.43 (.25)
                                                                                                              p-value = .09
QLT Y and P ROD cannot be constructed using only the
factors measured in this study, and therefore we use AIC                          REF                           −.25 (.11)
as the basis for the selection criterion. We are looking for                                                  p-value = .03
the model with the minimum AIC4 .                                                 Constant                     77.70 (4.05)
   Applying backwards selection, after three iterations                                                       p-value = .000
we obtain the models presented in Table 9 for QLT Y                               Adjusted R2 [conf. int.]     .12 [.01, .27]
and Table 10 for P ROD. These two models represent the                            Residual Std. Error         17.98 (df = 78)
                                                                                  F Statistic                4.86 (df = 3; 78)
best compromise between goodness of fit and simplicity                            p-value                           .003
with minimum loss of information.
   Both models are significant and exhibit an adjusted R2
indicating a medium effect size. Notice that SEQ and its
interaction with REF (SEQ:REF ) have been dropped                       outcome variables seems counterintuitive. One reason
from both models. This is surprising as it implies the                  may lie in the metric REF used to measure refactoring
sequence in which writing test and production code                      effort. REF may not reliably measure the construct that
are interleaved is not a prominent feature. The finding                 it targets, which raises a construct validity threat. This
counters the common folklore within the agile software                  has to do with the measure’s inability to differentiate
development community.                                                  between useful and harmful variants of refactoring. Most
   The factors related to the higher-level CY CLE compo-                cycles detected as refactoring cycles could have been
nent (GRA and U N I) as well as remainin T Y P E compo-                 associated with floss refactoring [38], a practice that mixes
nent (refactoring effort, REF ) are negatively associated               refactoring with other activities. A typical example in-
with both outcomes. In the end, the models for QLT Y                    volves sneaking in production code that implements new
and P ROD involve exactly the same factors.                             functionality while refactoring. The developer is peform-
   That refactoring has a negative relationship with both               ing refactoring, realizes that a piece of functionality is
                                                                        missing and mixes in the proper production code, but
 4. The theoretical best value for AIC is minus infinity.               the code is not covered by any tests, possibly causing a
                                                                                                                        13


TABLE 10: Regression summary for the P ROD model              model for the corresponding factor. This is the compo-
having the best AIC (-239.30). Standard error reported        nent line. The circles represent the partial residuals. The
in parentheses.                                               solid curve is fitted to the residuals using the LOESS
                                                              method—a standard, nonparametric method to fit a
                                    Outcome variable:
                                                              curve to a set of observations based on locally weighted
                                         PROD
                                                              polynomial regression [40]. A systematic departure of
            GRA                       −.003 (.002)
                                     p-value = .11            the residuals curve from the component line indicates
                                                              a poor fit [41], which does not appear to be the case in
            UNI                      −.003 (.002)             Figures 5 and 6. We observe sporadic local departures of
                                     p-value = .08
                                                              the LOESS curves from the component lines, especially
            REF                      −.003 (.001)             at some extremes, but the curves do not ”run away”
                                     p-value = .02            persistently.
            Constant                    .39 (.05)
                                     p-value = .000
            Adjusted R2               .10 [.01, .28]          5.3.3   Interpretation of Models
            Residual Std. Error       .22 (df = 78)
            F Statistic             4.14 (df = 2; 78)
            p-value                        .008               We briefly interpret the significant coefficients in the two
                                                              models.
                                                                 Regarding the GRA coefficient in QLT Y (Table 9),
feature interaction and introducing a hidden bug. Floss       reducing the cycle length further from the often sug-
refactoring is believed to be potentially harmful, nega-      gested values of five to ten minutes results in little
tively affecting quality and productivity. Pure refactoring   improvement. The improvement can be as high as 14%
cycles are difficult to detect accurately without before-     when cycle length is reduced from its maximum ob-
and-after code coverage comparison, which the process         served value at around 50 minutes down to the average
instrumentation tool we used did not perform.                 observation of eight minutes in the nominal range of the
                                                              dataset.
5.3.2 Assumption Diagnostics                                     With respect to REF coefficient, we have similar
We validate the assumptions for the models by running         observations, however, we have to qualify them with
the regression diagnostic proposed by Pena and Slate          the possibility that the detected refactoring cycles might
[39].                                                         have been dominated by the harmful variety (further dis-
                                                              cussed in the next section), hence the negative coefficient.
TABLE 11: Multiple regression diagnostics for QLT Y           Limiting refactoring effort from the average level (23%)
model in Table 9.                                             to its first quartile mid-point (9%), and similarly from the
                            Value    p-value    Decision      third quartile midpoint to the average level, results in
       Skewness              1.93        .16    Acceptable.   less than 4% improvement in external quality. However,
       Kurtosis               .20        .65    Acceptable.
       Link Function         1.63        .26    Acceptable.
                                                              a reduction from the extreme case (70%) to the average
       Heteroscedasticity     .10        .74    Acceptable.   case results in about 12% improvement.
                                                                 In the P ROD model (Table 10), only REF has a
                                                              significant coefficient, but again it is subject to the con-
TABLE 12: Regression diagnostics for P ROD model in           struct caveat of being possibly overrepresented by the
Table 10.                                                     harmful variety of refactoring. Keeping this qualification
                            Value    p-value    Decision      in mind, doing less refactoring is associated with an
       Skewness              7.21        .07    Acceptable.   increase in productivity. This observation is not sur-
       Kurtosis               .81        .36    Acceptable.
       Link Function         1.49        .22    Acceptable
                                                              prising since spending more time on refactoring should
       Heteroscedasticity    2.26        .13    Acceptable.   imply spending less time on adding new features, thus
                                                              reducing productivity. In particular reducing refactoring
  As reported in Tables 11 and 12, the models overall         effort from 70% of the time (the maximum observed
meet their statistical assumptions (p-value > α-level).       value) to the average case in the dataset—a reduction of
When a model includes several factors, partial residuals      ∼45%—results in a productivity improvement of 18%.
plots are helpful in visualizing the relationships between       In summary, the final optimal models include the
the factors and the outcome variable one at a time.           factors for granularity, uniformity, and refactoring ef-
Figures 5 and 6 show the Component and Compo-                 fort, but not sequencing. They also omit interactions
nent+Residuals (CCPR) plots for the QLT Y and P ROD           between these factors. However, at this point the role of
models, respectively. The CCPR plot is a variation of         uniformity vis-á-vis external quality and of granularity
a partial residual plot where the outcome values and          vis-á-vis productivity are still uncertain since the corre-
the partial residuals are plotted against a chosen factor.    sponding coefficients are insignificant in the underlying
The dashed line in each plot represents the fitted linear     models, although the models themselves are significant.
                                                                                                                         14




                                        Fig. 5: CCPR plots for model in Table 9


6     T HREATS TO VALIDITY                                     study is subject to single-group threats. The factors, or
In this section, we discuss the threats to the validity        independent variables, represent dimensions of applying
applicable to our study following the classification by        a particular type of development process. We are inter-
Wohlin et al. [42]:                                            ested in how the variation in the strength of the property
  1) Internal validity: threats that can lead to a misinter-   described by each dimension of the process affects the
     pretation of the relationship between independent         outcome measures. The dimensions cannot be controlled,
     and dependent variables.                                  as they arise naturally from applying a taught technique
  2) External validity: threats that can limit the appli-      on a best effort basis. They are neither all absent nor
     cability of the results to a greater extent, e.g., in     all present, but rather present to varying extents in a
     industrial practice.                                      continuum. This is the reason this study uses correlation
  3) Construct validity: threats that relate to how faith-     and regression analysis rather than an experimental de-
     fully the measures capture the theoretical con-           sign with defined control and experimental groups. We
     structs and how suitably the constructs are oper-         noted in the beginning that this study was embedded in
     ationalized to preserve their integrity.                  a larger ongoing study structured as a quasi-experiment.
  4) Conclusion validity: threats that concern the             However, the treatment and control groups, TDD and
     soundness of the statistical inferences made.             ITL, defined in that larger context, do not apply to this
                                                               study, as we generalized the groups to a higher-level
  The threats to the validity are presented in decreas-
                                                               process in which TDD and ITL are specific, idealized
ing order of priority following the recommendations of
                                                               cases. We are interested in the elementary dimensions of
Wohlin et al. [42]. We recognize that there are trade-offs
                                                               that high-level process. We acknowledge the limitations
between the different threats and focus on the types that
                                                               of our design in terms of causality. Our main goal was
are more important for applied research. Therefore, we
                                                               to identify the most salient of those dimensions in terms
give higher priority to the interpretation of the results
                                                               of their potential consequences.
and their applicability to relevant populations.
                                                                 History and maturation effects are not a concern for
6.1   Internal Validity                                        this study since they apply to all subjects equally. Threats
By the nature of its design, this is a single-group study.     related to testing and statistical regression to the mean
Therefore, there is no notion of a control group. The          are not applicable either due to the design.
                                                                                                                       15




                                      Fig. 6: CCPR plots for model in Table 10


   We mitigated the instrumentation threat by using mul-      level spanned a rather narrow range skewed towards
tiple tasks. Two of these tasks were selected from previ-     the medium- to high-experience end. These two consid-
ous studies, and a more complex task was introduced           erations limit the generalizability of the results to sub-
to increase the variation in observations. We needed          populations with profiles that differ from that of the
observations to be as diverse as possible.                    study.
   The drop-out rate of 10%, together with 8% incom-
plete or excluded observations, may have eliminated a           The tasks consisted of fixed user stories, all known
relevant subgroup of subjects. This in turn may have          upfront to the subjects. We acknowledge that this may
skewed the dataset.                                           not be the case in real world settings. When requirements
   A selection threat exists due to convenience sampling.     may change or not well known, leading the cycles with
Our sample was based on volunteers and company-               tests can provide advantages that were not detectable in
selected subjects. Such subjects tend to have different       this study: test-first can help to understand the require-
motivations and incentives different from those of ran-       ments, discover new/missing requirements, and find
domly selected subjects. In general, volunteers tend to       and fix requirements bugs.
be more enthusiastic and motivated. The converse might
apply to company-selected subjects, which in addition            Two of the three tasks used in the study were artificial
poses a social threat. The skill and experience profiles      and not particularly representative of a real-word situa-
of the two groups may have been skewed compared               tion for a professional software developer. This poses a
to a random sample. These design choices could have           threat to the generalizability of our results. To mitigate
influenced the results. However, we needed to accept the      this concern and increase realism, we included a third
compromises in the selection process because recruiting       task, and the three tasks together provided sufficient
professionals is inherently difficult, and when possible,     diversity and balance between algorithmic and architec-
often subject to selection bias [17].                         tural problem solving. The resulting diversity moderated
                                                              the threat to external validity.
6.2   External Validity                                         To minimize the interaction between setting and study
The participants in the study do not represent the en-        goals, the physical setting was kept as natural as possible
tire population of software developers. Specifically, their   for the participants. The subjects worked at their own
skills in unit testing was limited. Professional experience   workstations throughout the study.
                                                                                                                         16



6.3   Construct Validity                                       vational regression study with no controlled factors is
The study constructs were well defined, except possibly        however less important.
for the refactoring dimension. The factors granularity,           We believe the application domain in which the partic-
uniformity, and sequencing are based on straightfor-           ipants worked (security and gaming) was not a concern
ward, mechanical definitions, and were accordingly mea-        either for conclusion validity. The tasks were equally
sured by automated tools with straightforward heuris-          new and different to all participants.
tics. Refactoring effort is more special since the under-         The assumptions of the statistical tests used were
lying construct is difficult to capture, or even define        verified for all the models. In the final models, the
clearly. It was measured using an approximation. In the        effect sizes—represented by the adjusted R2 —fell in the
future, its detection may be made more accurate by more        medium range [29]. This level is acceptable in the study’s
sophisticated analysis of developer actions—for exam-          context because human behavior can introduce a great
ple through before-and-after code coverage analysis to         deal of unexplained variation [32] that may be impos-
better differentiate pure refactoring from the potentially     sible to account for. We remark that over 85% of the
harmful, mixed kind known as floss refactoring [27]. Since     variation in the outcome variables was still unaccounted
the corresponding factor, REF , appears in the final           for. However, the study was not focused on those factors;
regression models, a construct threat arises. In particular,   rather, the goal was to identify the most important ones
the role of refactoring in external quality deserves deeper    among four specific candidates.
investigation.
   The outcome variables were based on measures used           7   R ELATED W ORK
in previous studies. We had no reason to doubt their
ability to capture the underlying constructs in a rea-         The effects of TDD have been extensively studied. The
sonable way. The productivity measure was, by design,          evidence accumulated is synthesized in several system-
intended to capture short-term productivity. Therefore,        atic literature reviews (Table 13).
given the short duration of the tasks, it is possible that        Turhan et al. [43] reviewed controlled experiments,
the participants did not achieve sufficient maturity for       quasi-experiments and case studies of different scales in
robust measurement. This could suggest an interaction          both industry and academia. Their results suggest that
between maturity and construct validity.                       TDD overall has a moderate positive effect on external
   The outcome variables exhibit mono-method bias. We          quality, although this result becomes weaker when only
used a single measure to capture each outcome construct.       the most rigorous studies are considered. The results are
For example, the quality measure was based exclusively         contradictory for productivity overall, with controlled
on functional correctness. Other important aspects of          experiments exhibiting a weak positive effect.
external quality, in particular usability and usefulness,         A meta-analysis by Rafique and Misic [14] reports
were disregarded. Not triangulating the result with mul-       that TDD improves external quality when compared
tiple measures reduces the validity. We are still looking      to a waterfall approach. However, this improvement is
for alternative, objective, and reliable means of measur-      not strong. Further, TDD becomes disadvantageous for
ing these constructs.                                          the subset containing only academic studies in which
   Finally, the factors we studied are directly associated     it is compared to an iterative, test-last (ITL) process
with only two particular outcome constructs, external          instead of a waterfall approach. This result suggests
quality and developer productivity. The factors may            that sequencing might have a negative effect on exter-
also have an association with other important outcome          nal quality, which we haven’t observed. Productivity
constructs, such as internal quality (maintainability and      results are more inconclusive in that the authors report
understandability) and ability to deal with volatile re-       a small productivity hit for TDD when comparing TDD
quirements. These were not addressed. The factors’ im-         with waterfall (attributed to differences in testing effort,
pact on these other constructs can be significant, and         with a larger hit for the industrial subgroup), but the
thus worth investigating.                                      effect, even though still small, is reserved when ITL is
                                                               compared with TDD. This latter observation suggests
                                                               sequencing might have a positive influence on produc-
6.4   Conclusion Validity                                      tivity, which we have not been able to confirm, either.
Subject variability was desirable because this was a           Rafique and Misic recommend further investigation with
regression study. The participants were professionals          longer-term studies with larger tasks to allow the effects
from only two companies, therefore our sample was not          to become more visible. They also emphasize the impor-
very heterogeneous. There were few subjects who were           tance of process conformance in TDD, which we capture
experts with both relevant skills. The majority of the         using the four dimensions.
participants had to learn the required skills to different        According to a subsequent systematic literature review
extents during the workshops, which possibly further           by Munir et al. [44], TDD is beneficial for external
reduced variability in the sample.                             quality when only high-quality studies are considered.
   Skill and experience level are often confounding fac-       However, this improvement disappears over the whole
tors in experimental designs. Their role in an obser-          dataset, which includes studies of different levels of
                                                                                                                           17


                                  TABLE 13: Summary of secondary studies about TDD
                 Reference   Primary   Result                    Result                    Notes
                             Studies   External Quality          Productivity
                 [43]        32        Some improvement          Inconclusive              Following TDD
                                                                                           not enforced
                 [44]        41        Improvement in            Inconclusive              Inconclusive when
                                       high-rigor,high-                                    studies are considered
                                       relevance                                           together
                 [14]        27        Small improvements,       Inconclusive,             Task size
                                       more accentuated in in-   with a drop in industry   is a significant modera-
                                       dustry                                              tor



rigour. Again, the productivity results are inconclusive.            Pancur et al. [13] carried out two controlled experi-
The authors recommend future studies to focus on in-              ments comparing TDD to ITL, while making sure that
dustrial settings and professionals, and our study is in          the process granularity of two treatments were com-
that category.                                                    parable. Their rationale was to eliminate the possible
   Causevic et al.’s review [10] identified what they refer       effect of the granularity, thereby isolating the effect of
to as insufficient adherence to protocol, or process confor-      sequencing. Again, they could not produce any evidence
mance (i.e., the extent to which the steps of TDD are             that sequencing affects productivity or external quality.
followed), as one of the factors leading to the abandon-             Pancur et al. thus speculate that studies showing the
ment of the practice. The authors’ review of industrial           superiority of TDD over a test-last approach are in reality
case studies showed that, although developers deviate             due to the fact that most of the experiments employ
from the process in several situations, conformance to            a coarse-grained test-last process—closer to the Waterfall
the TDD process is regarded as important. The authors             approach—as control group. This creates a large dif-
suggest that industrial investigations of TDD consider            ferential in granularity between the treatments. In the
the underlying process, which is what we endeavored               end, it is possible that TDD performs better only when
to do in our study.                                               compared to a coarse-grained development process [13].
   Therefore, in general, there is a consensus that the no-
tion of process deserves a closer look. We agree with this        8     D ISCUSSION
view. However, often TDD researchers equate process               In this section, we answer the research questions and
with the test-first dynamic of TDD. We think that the             briefly discuss the implications. To answer the research
idea of process conformance [44] or adherence to protocol         questions, we rely on the results of the multiple regres-
[45] is not only limited to the sequencing dimension.             sion analysis, focusing on the models presented in Tables
Indeed, by looking closer at the other dimensions, we             9 and 10 after feature selection.
discover that granularity and uniformity of the devel-               We remark first that the final models are highly signif-
opment cycles trumps sequencing, at least in terms of             icant, and have a medium-sized adjusted R2 . However,
external quality, and developer productivity. In fact, in         the limitations presented in Section 6 should not be
our previous studies, we investigated the relationship            ignored in operationalizing the results.
between sequencing and external quality and sequenc-                 RQ-QLTY—Which subset of the four factors best ex-
ing and productivity [19], [46] for TDD-like processes,           plain external quality? Improvements in external qual-
including ITL. We did not find any evidence of such a             ity were associated with granular (short) development
relationship.                                                     cycles with minimal variation. Refactoring, counter-
   Müller and Höfer [12] investigated the ways experts          intuitively, had a negative association with external qual-
and novices differ in applying TDD, focusing on pro-              ity. Most notably, sequencing—the dimension which is
cess conformance. To our knowledge, they are the only             most commonly identified with TDD—was absent in the
researchers who explicitly considered characteristics of          model. There were no significant interactions between
TDD cycles other than sequencing. They used an auto-              granularity and uniformity. We conclude that granular-
matic inference tool similar to Besouro [22], which was           ity, uniformity and refactoring effort together constitute
used in our study. Similar to our approach, Müller and           the best explanatory factors for external quality.
Höfer use this tool to log developer actions and roll them          RQ-PROD—Which subset of the four factors best
into cycles with different levels of compliance to a set of       explain developer productivity? The results for produc-
TDD rules. Remarkably, they found that average cycle              tivity were similar to those of quality. Improvements
length, close to our definition of granularity, and the           in productivity were also associated with the factors
cycle length’s variation, close to our definition of unifor-      related to both cyclical characteristics of the process—
mity, were the most important differentiators between             granular cycles with minimal variation—and refactor-
experts and novices. In our study, we found a large and           ing effort. Sequencing and interactions were absent. We
highly significant correlation between granularity and            conclude that granularity, uniformity and refactoring
uniformity, suggesting that these two dimensions might            effort together constitute the best explanatory factors for
capture the same information.                                     developer productivity.
                                                                                                                                      18



   Given the study limitations and the answers to the           the quality and productivity effects of refactoring activity
research questions, we operationalize the results in terms      in the context of TDD-like processes. Such effects should
of the following take-aways.                                    be visible once the risk of breaking the system becomes
   Granularity and uniformity are possibly the most             a significant risk and the test assets start paying off in
important factors in TDD-like processes. According              terms of the coverage required to support adding new
to our results, emphasizing the iterative nature of the         functionality. Moreover the effects, harmful or beneficial,
process provides the “best bang for the buck”. We thus          possibly depend on the nature and distribution of the
recommend focusing on breaking down development                 refactoring activity—e.g., whether pure or floss–and thus
tasks into as small and as uniform steps as possible.           the construct must be represented by measures that are
We think that this aspect should be emphasised over             able to differentiate between the different types.
religiously focusing on leading each production cycle             In summary, dissecting the TDD process into its more
with unit tests. Our results confirm the common advice          elementary aspects shed light on certain established TDD
of limiting the length of production (test-code or code-        myths. Our results suggest that the secret of TDD, in
test) and refactoring cycles to five to ten minutes and         particular with respect to quality, might not be centered
keeping a steady rhythm.                                        on its test-first nature, but rather on its ability to encour-
   Short cycles and a steady rhythm go hand in hand,            age developers to consistently take fine-grained steps,
meaning these two characteristics together make a differ-       what Kent Beck calls baby steps [2], provided that they
ence. In isolation, they may not be as effective since some     keep writing tests. Thus TDD, and its variants, can be
of the individual coefficients in the regression models         thought of as a process that facilitates a highly fine-
were insignificant. Further studies should focus on the         grained approach to software development, which in
isolated effects of granularity and uniformity, specifically    turn promises to improve quality and productivity. Such
on developer productivity.                                      improvements may be small or uncertain in the short
   The order in which unit tests are written vis-á-vis         term. Further studies are needed to investigate more
production code does not appear to be important for             persistent, long-term effects and the role of refactoring.
TDD-like processes. This finding goes against the com-            An important corollary of our findings is that in-
mon folklore about TDD that stresses leading production         cremental test-last (ITL) and TDD are not necessarily
cycles with unit tests, above other aspects. Sequencing         that different deep down: they could be substitutes and
does not appear among the factors selected for either           equally effective provided that they are performed at the
external quality or productivity in the final models.           same level of granularity and uniformity. Rafique and
   The absence of sequencing as an influential dimension        Misic [14], Causevic et al. [10], [12], and Pancur et al. [13]
does not imply that a strictly develop-then-test (test-last)    previously speculated on the importance of granularity,
strategy should be preferred over a test-first strategy: this   as discussed in Section 7. Our findings support their
advice would require a negative (statistically significant)     intuition.
coefficient, which the models did not produce. Our result         We encourage other researchers to attempt to replicate
simply states that the order in which unit tests and            this study, preferably in varying contexts and with dif-
production code are written may not be as important             ferent subject profiles. A lab package [25] is available for
as commonly thought so long as the process is iterative,        replication purposes.
granular, and uniform. Therefore, the developers could
follow the approach they prefer while paying particular         ACKNOWLEDGMENTS
attention to step size and keeping a steady rhythm.             This research is supported in part by the Academy of
   We further qualify this latter advice with the limita-       Finland Project #278354 and the TEKES FiDiPro Pro-
tions of the study in mind. A test-first dynamic may            gramme. We would like to acknowledge Dr. Lucas Lay-
provide long-term advantages not addressed by or de-            man, who designed one of the tasks used in the study.
tected in our study. For example we have not tackled the        We are indebted to the reviewers whose diligence, thor-
potential benefits of test-first in resolving requirements      oughness and attention to detail were extremely helpful.
uncertainty, formalizing design decisions, and encourag-        They identified many mistakes and led us to uncover
ing writing more tests [6], all of which may kick in the        inconsistencies in the data. In particular, through their
longer term and tip the balance in favor of an emphasis         suggestions and insights, we were able to revise and
for test-first.                                                 improve our measures and analyses considerably.
   We are not able to draw general or sweeping con-
clusions regarding refactoring effort due to the validity
threat associated with the representation of this construct
                                                                R EFERENCES
and the short-term nature of the study. In our study,           [1]   R. Jeffries and G. Melnik, “TDD–The art of fearless program-
                                                                      ming,” Software, IEEE, vol. 24, no. 3, pp. 24–30, 2007.
refactoring effort was negatively associated with both          [2]   K. Beck, Test-driven development: by example, ser. The Addison-
external quality (an unexpected finding) and developer                Wesley signature series. Addison-Wesley, 2003.
productivity (an expected finding). However these find-         [3]   R. C. Martin, Agile software development: principles, patterns, and
                                                                      practices. Prentice Hall PTR, 2003.
ings were subject to limitations. We believe that longer-       [4]   R. Jeffries, A. Anderson, and C. Hendrickson, Extreme program-
term studies with better constructs are required to reveal            ming installed. Addison-Wesley Professional, 2001.
                                                                                                                                                          19



[5]  K. Schwaber, “Scrum development process,” in Business Object             [27] M. Aniche and M. Gerosa, “Most common mistakes in test-driven
     Design and Implementation. Springer, 1997, pp. 117–134.                       development practice: results from an online survey with devel-
[6] H. Erdogmus, M. Grigory, and J. Ron, “Test-driven development,”                opers,” in Software Testing, Verification, and Validation Workshops
     in Encyclopedia of Software Engineering, P. Laplante, Ed. New York:           (ICSTW), 2010 Third International Conference on, April 2010, pp.
     Taylor and Francis, 2009, pp. 1211–1229.                                      469–478.
[7] M. Fowler, Refactoring: improving the design of existing code. Pear-      [28] E. McCrum-Gardner, “Which is the correct statistical test to use?”
     son Education, 2009.                                                          British Journal of Oral and Maxillofacial Surgery, vol. 46, no. 1, pp.
[8] P. Weißgerber and S. Diehl, “Are refactorings less error-prone than            38–41, Jan. 2008.
     other changes?” in Proceedings of the 2006 international workshop on     [29] P. D. Ellis, The essential guide to effect sizes: Statistical power, meta-
     Mining software repositories. ACM, 2006, pp. 112–118.                         analysis, and the interpretation of research results.          Cambridge
[9] G. Bavota, B. De Carluccio, A. De Lucia, M. Di Penta, R. Oliveto,              University Press, 2010.
     and O. Strollo, “When does a refactoring induce bugs? an em-             [30] K. P. Burnham and D. R. Anderson, “Multimodel inference un-
     pirical study,” in Source Code Analysis and Manipulation (SCAM),              derstanding AIC and BIC in model selection,” Sociological methods
     2012 IEEE 12th International Working Conference on. IEEE, 2012,               & research, vol. 33, no. 2, pp. 261–304, 2004.
     pp. 104–113.                                                             [31] J. Cohen, “A power primer.” Psychological bulletin, vol. 112, no. 1,
                                                                                   p. 155, 1992.
[10] A. Causevic, D. Sundmark, and S. Punnekkat, “Factors limiting
                                                                              [32] V. B. Kampenes, T. Dybå, J. E. Hannay, and D. I. Sjøberg, “A
     industrial adoption of test-driven development: A systematic
                                                                                   systematic review of effect size in software engineering experi-
     review,” in Software Testing, Verification and Validation (ICST), 2011
                                                                                   ments,” Information and Software Technology, vol. 49, no. 11, pp.
     IEEE Fourth International Conference on. IEEE, 2011, pp. 337–346.
                                                                                   1073–1086, 2007.
[11] H. Erdogmus, M. Morisio, and M. Torchiano, “On the effective-            [33] D. E. Farrar and R. R. Glauber, “Multicollinearity in regression
     ness of the test-first approach to programming,” IEEE Transactions            analysis: the problem revisited,” The Review of Economic and
     on Software Engineering, vol. 31, no. 3, pp. 226–237, 2005.                   Statistics, vol. 49, pp. 92–107, 1967.
[12] M. M. Müller and A. Höfer, “The effect of experience on the            [34] J. Jobson, Applied Multivariate Data Analysis: Regression and Exper-
     test-driven development process,” Empirical Software Engineering,             imental Design (Springer Texts in Statistics). Springer, 1999.
     vol. 12, no. 6, pp. 593–615, 2007.                                       [35] H. Akaike, “A new look at the statistical model identification,”
[13] M. Pančur and M. Ciglarič, “Impact of test-driven development               Automatic Control, IEEE Transactions on, vol. 19, no. 6, pp. 716–723,
     on productivity, code and tests: A controlled experiment,” Infor-             Dec 1974.
     mation and Software Technology, vol. 53, no. 6, pp. 557–573, 2011.       [36] K. P. Burnham and D. R. Anderson, Model Selection and Multimodel
[14] Y. Rafique and V. B. Mišić, “The effects of test-driven development         Inference: A Practical Information-Theoretic Approach.           Springer,
     on external quality and productivity: A meta-analysis,” IEEE                  2003.
     Transactions on Software Engineering, vol. 39, no. 6, pp. 835–856,       [37] Y. Yang, “Can the strengths of AIC and BIC be shared? A con-
     2013.                                                                         flict between model indentification and regression estimation,”
[15] Y. Wang and H. Erdogmus, “The role of process measurement in                  Biometrika, vol. 92, no. 4, pp. 937–950, 2005.
     test-driven development,” in 4th Conference on Extreme Program-          [38] E. Murphy-Hill, C. Parnin, and A. P. Black, “How we refactor,
     ming and Agile Methods, 2004.                                                 and how we know it,” IEEE Transactions on Software Engineering,
[16] C. Mann, “Observational research methods. Research design                     vol. 38, no. 1, pp. 5–18, 2012.
     II: cohort, cross-sectional, and case-control studies,” Emergency        [39] E. A. Pena and E. H. Slate, “Global validation of linear model
     Medicine Journal, vol. 20, no. 1, pp. 54–60, 2003.                            assumptions,” Journal of the American Statistical Association, vol.
[17] A. T. Misirli, H. Erdogmus, N. Juristo, and O. Dieste, “Topic se-             101, no. 473, pp. 341–354, 2006.
     lection in industry experiments,” in the 2nd International Workshop.     [40] W. Cleveland and S. Devlin, “Locally weighted regression: an
     New York, New York, USA: ACM Press, 2014, pp. 25–30.                          approach to regression analysis by local fitting,” Journal of the
[18] D. Fucci and B. Turhan, “On the role of tests in test-driven                  American Statistical Association, vol. 83, no. 403, pp. 596–610, 1988.
     development: a differentiated and partial replication,” Empirical        [41] Y. Xia, “Model checking in regression via dimension reduction,”
     Software Engineering, vol. 19, pp. 1–26, 2013.                                Biometrika, vol. 96, no. 1, pp. 133–148, 2009.
[19] D. Fucci, B. Turhan, and M. Oivo, “Impact of process conformance         [42] C. Wohlin, Experimentation in software engineering: an introduction.
     on the effects of test-driven development,” in Proceedings of the 8th         Springer, 2000, vol. 6.
     ACM/IEEE International Symposium on Empirical Software Engineer-         [43] B. Turhan, L. Layman, M. Diep, H. Erdogmus, and F. Shull,
     ing and Measurement. ACM, 2014, p. 10.                                        “How effective is test-driven development?” in Making Software.
[20] L. Madeyski, “The impact of pair programming and test-                        O’Reilly Media, 2010.
     driven development on package dependencies in object-oriented            [44] H. Munir, M. Moayyed, and K. Petersen, “Considering rigor and
     design—an experiment,” Product-Focused Software Process Improve-              relevance when evaluating test driven development: A systematic
     ment, pp. 278–289, 2006.                                                      review,” Information and Software Technology, vol. 56, no. 4, pp.
                                                                                   375–394, Apr. 2014.
[21] A. Hernndez-Lpez, R. Colomo-Palacios, and A. Garca-Crespo,
                                                                              [45] A. Causevic, D. Sundmark, and S. Punnekkat, “Test case quality in
     “Productivity in software engineering: a study of its meanings for
                                                                                   test driven development: a study design and a pilot experiment,”
     practitioners: understanding the concept under their standpoint,”
                                                                                   in Evaluation Assessment in Software Engineering (EASE 2012), 16th
     in Information Systems and Technologies (CISTI), 2012 7th Iberian
                                                                                   International Conference on, 2012, pp. 223–227.
     Conference on, June 2012, pp. 1–6.
                                                                              [46] D. Fucci, B. Turhan, and M. Oivo, “Conformance factor
[22] K. Becker, B. Pedroso, and M. S. Pimenta, “Besouro: a framework               in test-driven development: Initial results from an enhanced
     for exploring compliance rules in automatic TDD behavior assess-              replication,” in Proceedings of the 18th International Conference
     ment,” Information and Software Systems Journal, 2014.                        on Evaluation and Assessment in Software Engineering, ser. EASE
[23] H. Kou, P. M. Johnson, and H. Erdogmus, “Operational definition               ’14. New York, NY, USA: ACM, 2014, pp. 22:1–22:4. [Online].
     and automated inference of test-driven development with Zorro,”               Available: http://doi.acm.org/10.1145/2601248.2601272
     Automated Software Engineering, 2010.
[24] B. George and L. Williams, “An initial investigation of test driven
     development in industry,” in Proceedings of the 2003 ACM sympo-
     sium on Applied computing. New York, NY, USA: ACM, 2003, pp.
     1135–1139.
[25] D. Fucci, “A dissection of test-driven development process:
     Does it really matter to test-first or to test-last? - lab
     package,” 6 2016. [Online]. Available: https://dx.doi.org/10.
     6084/m9.figshare.3458996.v5
[26] D. Fucci and B. Turhan, “A replicated experiment on the effective-
     ness of test-first development,” in 2013 ACM/IEEE International
     Symposium on Empirical Software Engineering and Measurement
     (ESEM). IEEE, 2013, pp. 103–112.
