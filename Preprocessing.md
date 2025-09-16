raw_combat = sc.read_h5ad("/mnt/data/raw/COMBAT-CITESeq-EXPRESSION-ATLAS.h5ad")
raw_data = combat_raw.layers["raw"]
adata = ad.AnnData(raw_data)
adata.obs = combat_raw.obs

adata.obsm = combat_raw.obsm

adata.var = combat_raw.var

adata.uns = combat_raw.uns

adata.write_h5ad("/mnt/data/raw/raw_combat.h5ad")

>>> adata.obs["major_subset"].isna().any()

np.False_

>>> adata.obs["minor_subset"].isna().any()

np.False_

adata_flrtd = adata[:,adata.X.count_nonzero(axis=0) > 3]
sum_x = adata_flrtd.X.sum(axis=0)
sum_x2= np.array(X.multiply(X).sum(axis=0))
n_cells = adata_flrtd.X.shape[0]

mean = sum_x / n_cells
mean = np.array(mean).flatten()
sum_x2 = sum_x2.flatten()
variances = (sum_x2 / n_cells) - (mean ** 2)

`top_genes_idx = np.argsort(variances)[-5000:][::-1]
`top_genes = adta_fltrd.var_names[top_genes_idx]

adata_flrtd_top5000 = adata_flrtd[:,top_genes_idx]
adata_flrtd_top5000.write_h5ad("/mnt/data/raw/combat_adata_fltrd_top5000genes.h5ad")