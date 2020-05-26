+++
title = "Fourier Analysis"
author = ["pp"]
lastmod = 2020-05-26T04:12:54+00:00
draft = false
+++

This is for my notes for Prof. Steve Brunton's [video lectures of
Fourier Analysis](https://www.youtube.com/watch?v=jNC0jxb0OxE&list=PLMrJAkhIeNNT%5FXh3Oy0Y4LTj0Oxo8GqsC).


## Real domain to complex domain {#real-domain-to-complex-domain}

The first confusion for me was the shift in the sum limits from real
domain to complex domain. The real fourier series is expressed as
\\(\sum\_{k=0}^{\infty} \frac{a\_0}{2} + a\_k \cos kx + b\_k \sin kx\\). The
intuition here is that harmonics can express any function. The
constant term accounts for all the different initial phase shifts of
each of these harmonics. The mathematical property of these
trigonometric terms \\(\sin(kx), \cos(kx)\\) is that they are orthogonal.
Their inner product \\(<f, g> = \int\_{-\pi}^{\pi} f(x) g^\*(x) dx =
0\\).

> To verify:
> The conjugation on the second term is irrelevant for real valued
> functions. \\(f,g\\) can be any \\(\sin(kx), \cos(kx)\\) term.
> [This lecture](https://ocw.mit.edu/resources/res-18-008-calculus-revisited-complex-variables-differential-equations-and-linear-algebra-fall-2011/study-materials/MITRES%5F18%5F008%5FpartIII%5Fsol08.pdf) goes into detail about each combination. The intuition is
> that \\(\int\_{-\pi}^{\pi} \sin ax \cos bx\\) is an odd function so
> integrating over this particular limit will be zero. But when \\(a=b\\)
> \\(\int \sin^2 ax, \int \cos^2 ax\\) are reduced using trigonometric
> identities to a constant plus a periodic function, and the constant
> part evaluates to \\(\pi\\).

All of the above is for the real domain. In complex domain, the
fourier series becomes: \\(\sum\_{k=-\infty}^{k=\infty} c\_k e^{ikx}\\)
where \\(c\_k \in \mathbb{C}\\). The sum starts from negative infinity now
unlike the real domain.


## Oscillations in real domain, rotations in complex domain {#oscillations-in-real-domain-rotations-in-complex-domain}

In real domain, the eigenfunctions are either \\(\sin kx\\) or
\\(\cos kx\\). This means that \\(\sin(kx)\\) and \\(\sin(-kx)\\) are not
orthogonal. This can be verified by the inner product which evaluates
to \\(- \pi\\).
So negative \\(k\\) in the real fourier series redundant as
the eigenfunctions of \\(k > 0\\) already include the \\(k < 0\\) part too.
Another way to think about this is phase shift. \\(k < 0\\)  functions are
simply a phase shift of the \\(k > 0\\)  terms. All the phase shift terms
are already accounted for by \\(a\_0\\).
But in complex domain, the eigenfunctions represent a rotation (at
different frequencies and starting points), rather
than oscillation. \\(k < 0\\)  here represents a clockwise rotation and a
\\(k > 0\\)  represents an anti-clockwise rotation.

> [This video](https://www.youtube.com/watch?v=spUNpyF58BY) helps

In this case, \\(k < 0\\) cannot be accounted for by \\(k > 0\\) functions. The
computation of the inner product confirms the same of course.

> TODO: The complex conjugation of the second term in the inner product is
> equivalent to a direction change of rotation. I know that the
> integration of a complex function is related to contour integration
> but I don't see how the inner product in the complex domain represents
> "closeness" of two rotations.
> [Yet
> to read this](http://www1.spms.ntu.edu.sg/~ydchong/teaching/08%5Fcontour%5Fintegration.pdf)
