<!--============ intro -->
# No more buffered points?
<!-- .slide: data-background="./gland_assets/cambridge.jpg" -->
<br>
<small>Yichuan Shi</small>

# The problem
<!-- .slide: data-background="./buf_points_assets/issue.png" -->

# Not possible 
<!-- .slide: data-background="./gland_assets/dogfail.gif" -->
to accurately calculate total area with points

<!-- .slide: data-background="http://i.giphy.com/LwAjTGdSWNRYc.gif" -->
# Not impossible
to estimate total area with reasonable confidence (make do with data!)

# Zero knowledge Ideas
- The ceiling (assuming no overlap): 
```python
sum(rep_area)
```
- The floor (assuming all overlap):
```python
0
```
- The true value must be in-between
```python
[0, sum(rep_area)]
```

# An attempt
- learn: current relationship between MPAs' overlap based known boundary
- predict: overlap of MPAs with points based on the learnt relationship

# Why it cannot be done
- the relationship cannot be modelled using non-spatial attributes alone
- permutation of boundaries is technically not possible

# model fitting and cross-validation

<i class="fa fa-spin fa-gear fa-5x"></i>
# The proposed solution
<!-- .slide: data-background="#1B5E20" -->

## point estimate
<!-- .slide: data-background="#1B5E20" -->
- those not possible to overlap (by search radius based on `rep_area`)
```python
sum(rep_area1)
```
- those possible to overlap 
```python
sum(rep_area2) * percentage
```
where `percentage` is calculated using MPAs ***with*** polygon boundary
```python
percentage = total_dissolved_area / sum(rep_area_gis_polygon)
```

## interval estimate
<!-- .slide: data-background="#1B5E20" -->
- The high confidence `total`
```python
TOTAL = total_gis_dissolve_polygon + sum(rep_area1)
```
- The ceiling consider all those possible to overlap ***does not*** overlap
```python
TOTAL + sum(rep_area2)
```
- The floor consider all those possible to overlap overlap
```python
TOTAL + 0
```

## search radius
<!-- .slide: data-background="#1B5E20" -->
Circular buffer is still the *best* guess for unknown boundary

## Implementation
<!-- .slide: data-background="#1B5E20" -->
```python
# calculate search radius based on rep_area
search_radius = sqrt(rep_area / PI)
# if a point overlaps with any polygon
# possible (must) to overlap
if point_overlap_with_polygon:
	overlap = True
# if a point's search radius is greater than distance to nearest polygon
# possible (very likely) to overlap
elif search_radius > distance_to_nearest_polygon:
	overlap = True
# if a point's search radius is shorter than distance to nearest polygon
# no overlap (very unlikely)
else:
	overlap = False
```

# Discussions
<i class="fa fa-child fa-5x"></i><i class="fa fa-child fa-5x"></i><i class="fa fa-child fa-5x"></i>
<!-- .slide: data-background="./gland_assets/cambridge.jpg" -->
