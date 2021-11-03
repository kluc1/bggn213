Untitled
================

# 1: Introduction to the RCSB Protein Data Bank (PDB)

``` r
Data_export_summary <- "Data Export Summary.csv"
PDB_statistics <- read.csv(Data_export_summary)
```

> Q1: What percentage of structures in the PDB are solved by X-Ray and
> Electron Microscopy.

``` r
X_ray <- sum(PDB_statistics$X.ray)/ sum(PDB_statistics$Total)
EM <- sum(PDB_statistics$EM)/sum(PDB_statistics$Total)
```

``` r
X_ray*100
```

    ## [1] 87.52836

``` r
EM*100
```

    ## [1] 4.94687

or we can do all columns!

> Q2: What proportion of structures in the PDB are protein?

``` r
PDB_statistics[1,]$Total/sum(PDB_statistics$Total)*100
```

    ## [1] 87.3499

> Q3: Type HIV in the PDB website search box on the home page and
> determine how many HIV-1 protease structures are in the current PDB?
> structures in the current PDB.

## The PDB format

# 2. Visualizing the HIV-1 protease structure

## Using Atom Selections

![](HIV%20Structure.png)

> Q4: Water molecules normally have 3 atoms. Why do we see just one atom
> per water molecule in this structure? This is because the image is
> created by X-ray crystallography with 2A resolution, hydrogens are too
> small to be seen at this resolution.

> Q5: There is a conserved water molecule in the binding site. Can you
> identify this water molecule? What residue number does this water
> molecule have (see note below)? residue number HOH308

> Discussion Topic: Can you think of a way in which indinavir, or even
> larger ligands and substrates, could enter the binding site?

# 3. Introduction to Bio3D in R

``` r
library(bio3d)
```

## Reading PDB file data into R

``` r
pdb <- read.pdb("1hel")
```

    ##   Note: Accessing on-line PDB file

``` r
pdb
```

    ## 
    ##  Call:  read.pdb(file = "1hel")
    ## 
    ##    Total Models#: 1
    ##      Total Atoms#: 1186,  XYZs#: 3558  Chains#: 1  (values: A)
    ## 
    ##      Protein Atoms#: 1001  (residues/Calpha atoms#: 129)
    ##      Nucleic acid Atoms#: 0  (residues/phosphate atoms#: 0)
    ## 
    ##      Non-protein/nucleic Atoms#: 185  (residues: 185)
    ##      Non-protein/nucleic resid values: [ HOH (185) ]
    ## 
    ##    Protein sequence:
    ##       KVFGRCELAAAMKRHGLDNYRGYSLGNWVCAAKFESNFNTQATNRNTDGSTDYGILQINS
    ##       RWWCNDGRTPGSRNLCNIPCSALLSSDITASVNCAKKIVSDGNGMNAWVAWRNRCKGTDV
    ##       QAWIRGCRL
    ## 
    ## + attr: atom, xyz, seqres, helix, sheet,
    ##         calpha, remark, call

> Q7: How many amino acid residues are there in this pdb object? 129

> Q8: Name one of the two non-protein residues? 185

> Q9: How many protein chains are in this structure? 1

``` r
attributes(pdb)
```

    ## $names
    ## [1] "atom"   "xyz"    "seqres" "helix"  "sheet"  "calpha" "remark" "call"  
    ## 
    ## $class
    ## [1] "pdb" "sse"

To access these individual attributes we use the dollar-attribute name
convention that is common with R list objects. For example, to access
the atom attribute or component use pdb$atom:

``` r
head(pdb$atom)
```

    ##   type eleno elety  alt resid chain resno insert      x      y      z o     b
    ## 1 ATOM     1     N <NA>   LYS     A     1   <NA>  3.294 10.164 10.266 1 11.18
    ## 2 ATOM     2    CA <NA>   LYS     A     1   <NA>  2.388 10.533  9.168 1  9.68
    ## 3 ATOM     3     C <NA>   LYS     A     1   <NA>  2.438 12.049  8.889 1 14.00
    ## 4 ATOM     4     O <NA>   LYS     A     1   <NA>  2.406 12.898  9.815 1 14.00
    ## 5 ATOM     5    CB <NA>   LYS     A     1   <NA>  0.949 10.101  9.559 1 13.29
    ## 6 ATOM     6    CG <NA>   LYS     A     1   <NA> -0.050 10.621  8.573 1 13.52
    ##   segid elesy charge
    ## 1  <NA>     N   <NA>
    ## 2  <NA>     C   <NA>
    ## 3  <NA>     C   <NA>
    ## 4  <NA>     O   <NA>
    ## 5  <NA>     C   <NA>
    ## 6  <NA>     C   <NA>

``` r
m <- nma(pdb)
```

    ##  Building Hessian...     Done in 0.022 seconds.
    ##  Diagonalizing Hessian...    Done in 0.12 seconds.

``` r
plot(m)
```

![](Class11_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
mktrj(m, file="nma.pdb")
```
