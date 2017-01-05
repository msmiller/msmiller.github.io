---
title: Fast Window Overlap Checking Algorithm
---

From deep in my archives ...

Many algorithms to check if two windows overlap (a very common activity in window system and interface design) rely on doing multiple comparisons of the positions of the sides of the two rectangular areas in question. This is usually fine for most instances, especially in these days of multi-gigahertz desktop PC's. But sometimes you need to do so many such comparisons that speed becomes the paramount concern. And sometimes you do not have a super fast processor to rely on - such as on small hand-held devices or cell-phones.

The algorithm described here is one I wrote back in 1988 for a graphics workstation product. It relies on some interesting relationships between the sign of the result of subtracting opposite sides of two rectangles. By taking advantage of this, it is possible to write few lines of C code which will compile down to 20 or fewer assembly instructions.

Simply put, if you subtract opposite sides of two rectangles the signs of the results will be the same if they do not overlap along that axis. So in pseudocode that comes out to:

```cpp
<pre lang="C" line="1">
if ( (sign(top1 - bottom2) != sign(bottom1 - top2)) &amp;&amp;
    (sign(left1 - right2) != sign(right1 - left2)) ) return(1);
```

This can be further optimized by using the XOR function, which allows the entire algorithm to be reduced to a 1-line preprocessor macro which has only one logical comparison - everything else is integer or bit-wise math. In this case all we are trying to do is get the high-order bit (which determines sign of the integer) to be set based on the subtractions, and then test it for the occlusion.

```cpp
typedef struct {
    int t, l, r, b;
} rect;   

#define test_olap (_r1, _r2)
    ( ( ( ( (_r1.t - _r2.b) ^ (_r1.b - _r2.t) ) &amp;
    ( (_r1.l - _r2.r) ^ (_r1.r - _r2.l) ) ) &lt; 0) ? 1 : 0)
```

Further optimizations could be done for a C/C++ coded version for early rejection based a negative return from the top/bottom or left/right comparison. But the gain would be negligable since that would require kicking out to logical operations and comparisons. The macro above compiled down to around 20 instructions on SPARC/680X0 series chips, and loading the registers for an early compare would not shave much off that.

If this has been of use to you, please let me know where and how you used it.
