### 📝 URA-F2022: Mathematical Handwriting Recognition Using Legendre-Sobolev Coefficients

**1. Project Overview**
This project focuses on developing a deep-learning system for online mathematical handwriting recognition (HWR). The core approach involves representing handwritten symbol curves in a low-dimensional vector space using coefficients from orthogonal polynomial series (specifically Legendre-Sobolev), enabling efficient and accurate real-time classification.

**2. Methodology & Technical Approach**

**2.1 Core Mathematical Foundation**
The method is grounded in functional analysis. A handwritten stroke's x- and y-coordinates are treated as functions, `X(λ)` and `Y(λ)`, parameterized by arc length `λ`.
*   **Inner Product & Basis:** An inner product is defined on the space of polynomials. The choice of inner product determines the orthogonal basis.
*   **Weight Function `w(t)`:** The function `w(t)` in the inner product integral acts as a weighting factor across the domain. Different weights (e.g., `w(t)=1` for Legendre, `w(t)=1/√(1-t²)` for Chebyshev) lead to different orthogonal polynomial families (Legendre, Chebyshev, etc.).
*   **Gram-Schmidt Orthogonalization:** The standard monomial basis `{1, t, t², ...}` is orthogonalized using the Gram-Schmidt process with respect to the chosen inner product to generate the specific orthogonal polynomial basis `{P₀(t), P₁(t), ..., P_d(t)}`.

**2.2 Legendre-Sobolev Representation**
To better capture curve shape (not just position), a **Legendre-Sobolev inner product** is used. It incorporates information about the function's derivative:
`<f, g> = ∫ f(t)g(t) dt + μ ∫ f'(t)g'(t) dt`
where `μ` is a experimentally determined constant. The resulting **Legendre-Sobolev coefficients** provide a compact, rich feature vector for each curve.

**2.3 Real-Time ("Online") Computation**
A significant innovation is computing coefficients *as the stroke is being written*.
1.  **Moment Calculation:** Numerical integrals (moments) of the form `∫ λᵏ X(λ) dλ` are updated with each new sampled point from the digitizer.
2.  **Pen-Up Calculation:** When the pen is lifted, the known arc length `L` is used to scale the domain to `[-1, 1]`. The Legendre-Sobolev coefficient vector is then computed from the stored moments in constant time, independent of the number of sampled points.

**2.4 Data Pipeline & Classification**
*   **Dataset:** The work uses a corpus of isolated mathematical symbols (e.g., the "InkDB" corpus). For this project, data is organized by symbol class (e.g., 38 samples for each of 26 capital letters).
*   **Preprocessing:** Each stroke trace is normalized: its centroid is translated to the origin, and it is scaled so its total arc length is 1.
*   **Feature Vector:** The preprocessed `X(λ)` and `Y(λ)` functions are represented by their truncated Legendre-Sobolev coefficient vectors.
*   **Classification:** These coefficient vectors reside in a Euclidean space. Classification can be performed by:
    *   **Distance-Based Methods:** Measuring the distance from an unknown sample to the convex hull of nearest neighbor classes (found to be ~97.5% accurate).
    *   **Support Vector Machines (SVM):** Training a classifier on the coefficient vectors.

**3. Key Tasks from Notes**
- Read and implement algorithms from the foundational papers.
- Process the ink data directory (`infCDB`), parsing ink files to organize data by symbol and stroke order.
- Write code to calculate Legendre-Sobolev coefficients for a given set of `(x, y)` points.
- Experiment with the calculated coefficient vectors for symbol classification.

### 📚 References in IEEE Format
Here are the core references for this methodology, cited in IEEE format with active links.

[1] O. Golubitsky and S. M. Watt, "Online stroke modeling for handwriting recognition," in *Proc. Conf. Center Adv. Stud. Collaborative Res. (CASCON)*, Toronto, ON, Canada, 2008, pp. 85–99. [Online]. Available: http://cs.uwaterloo.ca/~smwatt/pub/reprints/2008-cascon-moments.pdf

[2] O. Golubitsky and S. M. Watt, "Distance-based classification of handwritten symbols," *Int. J. Doc. Anal. Recognit.*, vol. 12, no. 2, pp. 133–146, Sep. 2009. [Online]. Available: http://cs.uwaterloo.ca/~smwatt/pub/reprints/2009-ijdar-similarity.pdf

[3] P. Alvandi and S. M. Watt, "Real-time computation of Legendre-Sobolev approximations," in *Proc. 20th Int. Symp. Symbolic Numeric Algorithms Sci. Comput. (SYNASC)*, Timisoara, Romania, 2018, pp. 167–174. [Online]. Available: http://cs.uwaterloo.ca/~smwatt/pub/reprints/2018-synasc-real-time-ls.pdf

[4] S. M. Watt, "The mathematics of mathematical handwriting recognition," presented at the *Theor. Issues Read. Instr. Comput. Syst. Semin. (TRICS)*, Univ. Western Ontario, London, ON, Canada, Sep. 15, 2010. [Online]. Available: https://cs.uwaterloo.ca/~smwatt/talks/2010-trics-math-of-mathhwr-talk.pdf

I hope this structured document serves as a clear foundation for your project. Would you like to delve deeper into any specific section, such as the algorithmic details of the Gram-Schmidt process for Legendre-Sobolev polynomials or the specifics of the convex hull classification method?