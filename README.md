# Kasghar

**Literature search and record management for systematic reviews, running entirely on your own computer.**
[Türkçe](#türkçe) · [English](#english)

## English

Kasghar searches Crossref, PubMed, OpenAlex, Europe PMC, Scopus, Semantic Scholar, CORE, ClinicalTrials.gov and DOAJ with one query, stores the results in a local database, links duplicate records into single works, and guides you through a systematic review with a PRISMA record at every step. Export to Zotero, Mendeley and EndNote (BibTeX, RIS, CSL JSON, EndNote XML, CSV). The interface is available in Turkish and English.

### Install and run

You need [Docker](https://docs.docker.com/get-docker/). One command downloads and starts everything; the database is inside the image.

```bash
docker run -d --name kasghar --restart unless-stopped --stop-timeout 60 -p 127.0.0.1:8787:8000 -v kasghar-data:/data ghcr.io/sadettindmr/kasghar:latest
```

Then open **http://localhost:8787**. Images are provided for `linux/amd64` and `linux/arm64` (Windows, macOS including Apple silicon, Linux).

| Task | Command |
|---|---|
| Stop / start | `docker stop kasghar` / `docker start kasghar` |
| Update | Back up first (Admin > Backup), then `docker pull ghcr.io/sadettindmr/kasghar:latest`, `docker rm -f kasghar`, and run the install command again |
| Use another port | Replace `8787` in the command |
| Save files to a folder | Add `-v ~/Kasghar:/exports -e EXPORT_DIR_CONTAINER=/exports -e EXPORT_DIR_HOST=~/Kasghar` |
| Use local AI models (Ollama) on Linux | Add `--add-host=host.docker.internal:host-gateway` |

Your data stays in the `kasghar-data` Docker volume. `docker volume rm kasghar-data` deletes it permanently. There is no login: keep `127.0.0.1` in the port option so that only your computer can reach the application.

### License

Copyright © 2026 Sadettin Demir. Licensed under the [PolyForm Noncommercial License 1.0.0](LICENSE).

- Free for personal use, research and study, and for educational institutions, public research organizations, health organizations, charities and other noncommercial organizations.
- Commercial use requires a separate license: [open an issue](https://github.com/sadettindmr/Kasghar/issues).
- If you use Kasghar in academic work, please cite: Demir, S. (2026). *Kasghar* [Computer software]. https://github.com/sadettindmr/Kasghar
- Third-party components in the image (PostgreSQL, Python, FastAPI and others) keep their own licenses; see the About window. Data from each source is subject to that source's terms.

## Türkçe

Kasghar; Crossref, PubMed, OpenAlex, Europe PMC, Scopus, Semantic Scholar, CORE, ClinicalTrials.gov ve DOAJ'da tek sorguyla arama yapar, sonuçları bilgisayarınızdaki veritabanında toplar, mükerrer kayıtları tek esere bağlar ve sistematik derlemeyi adım adım, PRISMA kaydıyla yürütür. Zotero, Mendeley ve EndNote'a aktarılabilir (BibTeX, RIS, CSL JSON, EndNote XML, CSV). Arayüz Türkçe ve İngilizcedir.

### Kurulum

[Docker](https://docs.docker.com/get-docker/) gerekir. Tek komut her şeyi indirip başlatır; veritabanı imajın içindedir.

```bash
docker run -d --name kasghar --restart unless-stopped --stop-timeout 60 -p 127.0.0.1:8787:8000 -v kasghar-data:/data ghcr.io/sadettindmr/kasghar:latest
```

Ardından **http://localhost:8787** adresini açın. Windows, macOS (Apple silicon dahil) ve Linux'ta çalışır. Ayrıntılı anlatım: [kullanım kılavuzu](KILAVUZ.md).

| İşlem | Komut |
|---|---|
| Durdur / başlat | `docker stop kasghar` / `docker start kasghar` |
| Güncelle | Önce yedek alın (Admin > Yedekleme); sonra `docker pull ghcr.io/sadettindmr/kasghar:latest`, `docker rm -f kasghar` ve yeniden kurulum komutu |
| Başka port | Komuttaki `8787`'yi değiştirin |
| Dosyaları klasöre kaydet | `-v ~/Kasghar:/exports -e EXPORT_DIR_CONTAINER=/exports -e EXPORT_DIR_HOST=~/Kasghar` ekleyin |

Veriler `kasghar-data` adlı Docker volume'ündedir; `docker volume rm kasghar-data` kalıcı olarak siler. Giriş (parola) yoktur: yalnızca kendi bilgisayarınızdan erişilsin diye `127.0.0.1`'i değiştirmeyin.

### Lisans

Copyright © 2026 Sadettin Demir. [PolyForm Noncommercial License 1.0.0](LICENSE).

- Kişisel kullanım, araştırma ve öğrenim; eğitim kurumları, kamu araştırma kurumları, sağlık kuruluşları, hayır kurumları ve diğer ticari olmayan kuruluşlar için serbesttir.
- Ticari kullanım ayrı lisans gerektirir: [issue açın](https://github.com/sadettindmr/Kasghar/issues).
- Akademik çalışmada kullandıysanız lütfen atıf verin: Demir, S. (2026). *Kasghar* [Bilgisayar yazılımı]. https://github.com/sadettindmr/Kasghar
- İmajdaki üçüncü taraf bileşenler kendi lisanslarına, kaynaklardan gelen veriler ilgili kaynağın koşullarına tabidir.

[Sürüm notları](CHANGELOG.md)
