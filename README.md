from zipfile import ZipFile
from pathlib import Path
import shutil

base=Path('/mnt/data/wedding_site_package')
(base/'images').mkdir(parents=True,exist_ok=True)

# Copy uploaded html
shutil.copy('/mnt/data/wedding-website.html', base/'index.html')

# Copy available images
imgs=['IMG_9534.jpeg','IMG_9531.jpeg','IMG_9530.jpeg','IMG_9036.jpeg','IMG_9035.jpeg']
for img in imgs:
    src=Path('/mnt/data')/img
    if src.exists():
        shutil.copy(src, base/'images'/img)

readme=base/'README.txt'
readme.write_text("""Wedding Website Package

Contents:
- index.html (base website)
- images/ (your uploaded photos)

Next steps:
1. Replace placeholder text in index.html with your final details if not already done.
2. Update image references in the gallery/hero.
3. Add your Google Form action/embed.
4. Drag and drop this folder or ZIP onto Netlify, or import into Vercel.

""")

zip_path='/mnt/data/Siphamandla_Vonani_Wedding_Website.zip'
with ZipFile(zip_path,'w') as z:
    for p in base.rglob('*'):
        z.write(p,p.relative_to(base))
print(zip_path)
