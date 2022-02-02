# Mahesh Kumar Nanganoori
* Hobbies
    * Playing cricket 
    * Decode some code snippets from online 
![Mahesh_Kumar](/Mahesh.jpg) 
*******************************************************************************
## Tables
|Sport|Location|Money|
|:---|:---:|---:|
|Cricket|INDIA|300$|
|Badminton|JAPAN|200$|
|Hockey|INDIA|200$|
|BasketBall|USA|100$|
***********************************************************************
> Be yourself and people will like you.
>> *~Jeff Kinney*
>
> It is better to be hated for what you are than to be loved for what you are not.
>> *~André Gide*
***********************************************************************************

> Algorithms that construct convex hulls of various objects have a broad range of applications in mathematics and computer science.

In computational geometry, numerous algorithms are proposed for computing the convex hull of a finite set of points, with various computational complexities.

Computing the convex hull means that a non-ambiguous and efficient representation of the required convex shape is constructed. The complexity of the corresponding algorithms is usually estimated in terms of n, the number of input points, and sometimes also in terms of h, the number of points on the convex hull.

[Link to Another Source](https://en.wikipedia.org/wiki/Convex_hull_algorithms)

[Link to Code Source](https://cp-algorithms.com/geometry/convex-hull.html)
  ```
  struct pt {
    double x, y;
};

int orientation(pt a, pt b, pt c) {
    double v = a.x*(b.y-c.y)+b.x*(c.y-a.y)+c.x*(a.y-b.y);
    if (v < 0) return -1; // clockwise
    if (v > 0) return +1; // counter-clockwise
    return 0;
}

bool cw(pt a, pt b, pt c, bool include_collinear) {
    int o = orientation(a, b, c);
    return o < 0 || (include_collinear && o == 0);
}
bool collinear(pt a, pt b, pt c) { return orientation(a, b, c) == 0; }

void convex_hull(vector<pt>& a, bool include_collinear = false) {
    pt p0 = *min_element(a.begin(), a.end(), [](pt a, pt b) {
        return make_pair(a.y, a.x) < make_pair(b.y, b.x);
    });
    sort(a.begin(), a.end(), [&p0](const pt& a, const pt& b) {
        int o = orientation(p0, a, b);
        if (o == 0)
            return (p0.x-a.x)*(p0.x-a.x) + (p0.y-a.y)*(p0.y-a.y)
                < (p0.x-b.x)*(p0.x-b.x) + (p0.y-b.y)*(p0.y-b.y);
        return o < 0;
    });
    if (include_collinear) {
        int i = (int)a.size()-1;
        while (i >= 0 && collinear(p0, a[i], a.back())) i--;
        reverse(a.begin()+i+1, a.end());
    }

    vector<pt> st;
    for (int i = 0; i < (int)a.size(); i++) {
        while (st.size() > 1 && !cw(st[st.size()-2], st.back(), a[i], include_collinear))
            st.pop_back();
        st.push_back(a[i]);
    }

    a = st;
}

```