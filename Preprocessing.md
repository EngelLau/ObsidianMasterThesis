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

