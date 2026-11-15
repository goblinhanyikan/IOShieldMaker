# I/O Shield Designer

**Language / Dil:** [English](#english) · [Türkçe](#türkçe)

---

<a id="english"></a>
## English

-WARNING: THIS APP IS VIBECODED. IF YOU ARE AVOIDING AI GENERATED CODE, YOU DESERVE TO KNOW-

Access it online here : https://goblinhanyikan.github.io/IOShieldMaker/

A single-file web app (`io-shield-designer.html`, no build step) that helps you design a 3D-printable PC I/O shield. You load a photo of your motherboard's rear I/O (or an existing shield), the app straightens it onto a plate of known size, and you place and fine-tune the port cutouts on top of it. It then exports an STL for printing.

> **Status:** not yet field-tested. Port presets are approximate. Measure your own board and case with calipers and print a quick test piece before committing to a full print.

### Quick start

1. Open `io-shield-designer.html` in a browser (or the published artifact link).
2. **1 · Align photo:** upload a photo, then drag the four numbered corners onto the corners of the I/O area (1 top-left, 2 top-right, 3 bottom-right, 4 bottom-left). Zoom with the slider or mouse wheel, drag empty photo space to pan, and use the **1–4** buttons to jump to a corner.
3. **2 · Design:** the photo is warped onto the plate. Use the **Photo opacity** slider to compare it with your layout. Add ports from the preset list, drag to move, drag the round handle to resize, or type exact values.
4. Export with **Save STL**, **Save STL (zip)** or **Save SVG**.

### Features

| Area | What it does |
|---|---|
| Photo alignment | Four-corner perspective correction, zoom (1–10×), pan, jump-to-corner |
| Orientation fixes | **Mirror photo**, **Flip vertically**, **Rotate 90°**. These change only how the photo is laid onto the plate, so you do not redo the corners |
| Overlay | Photo opacity slider; cutouts become more solid as the photo fades |
| Ports | Presets (USB-A, USB-C, RJ45, HDMI, DisplayPort, audio, PS/2, DVI, VGA, custom), drag, resize, corner radius, duplicate, delete, arrow-key nudge (0.05 mm, Shift = 0.5 mm) |
| Snapping | Snap to other ports (guides for edges and centers, works for diagonal neighbors); draw a line and snap ports onto it; **Snap all to line**; **Align all to selected row** |
| Measure | **Measure distance**: tap two ports to see center-to-center and edge-to-edge distances (X and Y) |
| Snap-fit rim | Flange (rim) around the body plus snap ledges on all four sides, so the shield can be pushed in from outside even with the motherboard mounted |
| Export | Binary STL, STL in a `.zip`, SVG outline, layout JSON (copy / load) |
| Languages | English and Turkish (button at the top; defaults to the browser language) |

### Plate parameters (mm)

| Field | Meaning | Default |
|---|---|---|
| Body width / height | The part that goes through the case opening | 158.75 × 44.45 |
| Body depth | How far the body extends into the case | 2 |
| Clearance | Extra gap added around every port cutout (per side) | 0.2 |
| Rim width / Rim thick | Flange that stays outside the case | 3 / 1.2 |
| Case metal | Thickness of your case's sheet metal (sets where the ledges sit) | 1 |
| Catch height / length / per side | Snap ledges: how far they stick out, how long, how many per long side (short sides get one) | 0.5 / 12 / 3 |

Set body width and height to your **case opening minus a small tolerance**, not the nominal ATX shield size. With the default 1 mm case metal and 2 mm body depth, only 1 mm is left for the snap ramp; increase body depth if the ledges feel too thin.

### Printing notes

- The model is oriented **flange-down**: the outside face lies on the bed, so no flipping is needed. The flange stays outside the case, the body goes through the opening, and the ledges catch behind the case metal.
- Curves are stair-stepped at 0.1 mm (`CONFIG.exportStep`). Your slicer may offer to repair tiny non-manifold edges; that is expected.
- For ledges that flex, use PETG or similar. If the fit is too tight or loose, change **Catch height** in 0.1 mm steps.

### Downloads: known limitation

The in-app save dialog only accepts certain file types, and **`.stl` is not one of them**. **Save STL** therefore tries the dialog, then a plain browser download; if neither produces a file, use **Save STL (zip)** (the zip contains `io-shield.stl`). A plain download should work when the page is opened in a normal browser tab. **Copy layout JSON** is also available as a fallback for regenerating the model elsewhere.

### Code map (for people and agents)

Everything lives in one HTML file with numbered sections at the top of the script:

| # | Section | Contents |
|---|---|---|
| 1 | `CONFIG` | Defaults: plate size, snap distances, export step, ramp steps, photo size limit |
| 2 | `PRESETS` | Port sizes `{ w, h, r }` in mm. **Add a connector by adding one line.** |
| 3 | `STATE` | The only mutable data (`state`); the UI is rendered from it. Also holds the `TR` translation map and `t()` |
| 4 | Geometry | `solveHomography`, `toMatrix3d` (photo → plate warp), `holeSpan` / `holesAt` (cutouts), `buildLayers` / `catchRects` (flange, body, snap ledges), `snapPosition` (snapping) |
| 5 | Export | `buildStl` (layered slab model), `zipStore` (single-file ZIP), `buildSvg`, `saveFile` (download wrapper) |
| 6 | Render | `renderAlign`, `renderDesign`, `rimSvg`, `overlaySvg`, `measureSvg`, field builders |
| 7 | Events | `bindDrag` (pointer dragging for both views), keyboard, inputs, buttons, `setLang` |
| 8 | Init | First render |

Conventions: plate space is millimetres with the origin at the top-left and y pointing down, as seen from outside the case. In the STL, `z = 0` is the outside face of the flange and `z` grows into the case.

**Adding a language:** add another map next to `TR` (keys are the English strings), extend `t()` and the toggle in `setLang()`. Static text is translated automatically by walking the DOM; text generated in JS must go through `t()`.

### Ideas / not done yet

3MF export · ports that can rotate · plate corner radius · screw tabs · pinch-to-zoom · auto-detecting ports from the photo · saving projects automatically.

---

<a id="türkçe"></a>
## Türkçe

-UYARI: BU UYGULAMA ÇATIR ÇATIR YAPAY ZEKAYA YAPTIRILMIŞTIR. HERHANGİ BİR NEDENDEN DOLAYI YAPAY ZEKADAN UZAK DURMAK İSTİYORSANIZ BUNU BİLMEK HAKKINIZ.-

Bu linkten online olarak kullanabilirsiniz: https://goblinhanyikan.github.io/IOShieldMaker/

Bilgisayar kasaları için 3D yazıcıyla basılabilir **I/O paneli (I/O shield)** tasarlamaya yarayan tek dosyalık bir web uygulaması (`io-shield-designer.html`, derleme gerekmez). Anakartınızın arka I/O bölgesinin (ya da mevcut bir panelin) fotoğrafını yüklersiniz; uygulama fotoğrafı bilinen ölçüdeki bir plakaya oturtur, siz de port kesimlerini üzerinde yerleştirip ince ayar yaparsınız. Sonunda baskı için STL dosyası alırsınız.

> **Durum:** henüz gerçek kullanımda denenmedi. Port ölçüleri yaklaşıktır. Kendi anakartınızı ve kasanızı kumpasla ölçün ve tam baskıdan önce küçük bir deneme parçası basın.

### Hızlı başlangıç

1. `io-shield-designer.html` dosyasını tarayıcıda açın (ya da yayınlanan bağlantıyı kullanın).
2. **1 · Fotoğrafı hizala:** fotoğraf yükleyin, sonra numaralı dört köşeyi I/O alanının köşelerine sürükleyin (1 sol üst, 2 sağ üst, 3 sağ alt, 4 sol alt). Kaydırıcı veya fare tekerleğiyle yakınlaştırın, boş fotoğraf alanını sürükleyerek kaydırın, **1–4** düğmeleriyle köşeye atlayın.
3. **2 · Tasarım:** fotoğraf plakaya oturtulur. **Fotoğraf saydamlığı** kaydırıcısıyla yerleşiminizi fotoğrafla karşılaştırın. Hazır listeden port ekleyin, taşımak için sürükleyin, boyutlandırmak için yuvarlak tutamacı sürükleyin ya da değerleri elle yazın.
4. **STL kaydet**, **STL kaydet (zip)** veya **SVG kaydet** ile dışa aktarın.

### Özellikler

| Alan | Ne yapar |
|---|---|
| Fotoğraf hizalama | Dört köşeli perspektif düzeltme, yakınlaştırma (1–10×), kaydırma, köşeye atlama |
| Yön düzeltmeleri | **Fotoğrafı aynala**, **Dikey çevir**, **90° döndür**. Yalnızca fotoğrafın plakaya yerleşimini değiştirir; köşeleri yeniden ayarlamanız gerekmez |
| Üst üste bindirme | Fotoğraf saydamlığı kaydırıcısı; fotoğraf soldukça kesimler daha belirgin olur |
| Portlar | Hazır ölçüler (USB-A, USB-C, RJ45, HDMI, DisplayPort, ses, PS/2, DVI, VGA, özel), sürükleme, boyutlandırma, köşe yarıçapı, çoğaltma, silme, ok tuşlarıyla kaydırma (0,05 mm, Shift = 0,5 mm) |
| Yapışma | Diğer portlara yapışma (kenar ve merkez kılavuzları, çapraz komşularda da çalışır); çizgi çizip portları üzerine yapıştırma; **Hepsini çizgiye yapıştır**; **Hepsini seçili sıraya hizala** |
| Ölçüm | **Mesafe ölç**: iki porta dokunun, merkezden merkeze ve kenardan kenara mesafeleri (X ve Y) görün |
| Geçmeli kenar | Gövdenin etrafında flanş (kenar) ve dört tarafta geçmeli çıkıntılar; anakart takılıyken bile panel dışarıdan itilerek yerine geçirilebilir |
| Dışa aktarma | İkili (binary) STL, `.zip` içinde STL, SVG taslağı, yerleşim JSON verisi (kopyala / yükle) |
| Diller | İngilizce ve Türkçe (üstteki düğme; varsayılan tarayıcı dilidir) |

### Plaka parametreleri (mm)

| Alan | Anlamı | Varsayılan |
|---|---|---|
| Gövde genişliği / yüksekliği | Kasa açıklığından geçen parça | 158,75 × 44,45 |
| Gövde derinliği | Gövdenin kasanın içine uzunluğu | 2 |
| Boşluk payı | Her port kesiminin etrafına eklenen boşluk (her kenar için) | 0,2 |
| Kenar genişliği / kalınlığı | Kasanın dışında kalan flanş | 3 / 1,2 |
| Kasa sacı | Kasa sacının kalınlığı (çıkıntıların yerini belirler) | 1 |
| Kanca yüksekliği / uzunluğu / kenar başına | Geçmeli çıkıntılar: ne kadar dışarı çıktığı, uzunluğu, uzun kenar başına adedi (kısa kenarlarda birer tane) | 0,5 / 12 / 3 |

Gövde genişliğini ve yüksekliğini standart ATX panel ölçüsüne göre değil, **kasa açıklığından küçük bir tolerans düşerek** ayarlayın. Varsayılan 1 mm kasa sacı ve 2 mm gövde derinliğinde geçme rampası için yalnızca 1 mm kalır; çıkıntılar ince gelirse gövde derinliğini artırın.

### Baskı notları

- Model **flanş aşağıda** duracak şekilde yönlendirilir: dış yüz tablaya oturur, çevirmeye gerek yoktur. Flanş kasanın dışında kalır, gövde açıklıktan geçer, çıkıntılar kasa sacının arkasına takılır.
- Eğriler 0,1 mm basamaklıdır (`CONFIG.exportStep`). Dilimleyici küçük hataları onarmayı önerebilir; bu beklenen bir durumdur.
- Esneyen çıkıntılar için PETG gibi bir malzeme kullanın. Geçme çok sıkı ya da gevşekse **Kanca yüksekliğini** 0,1 mm adımlarla değiştirin.

### İndirme: bilinen kısıtlama

Uygulama içi kaydetme penceresi yalnızca belirli dosya türlerini kabul eder ve **`.stl` bunlardan biri değildir**. Bu yüzden **STL kaydet** önce pencereyi, sonra düz tarayıcı indirmesini dener; ikisi de dosya üretmezse **STL kaydet (zip)** düğmesini kullanın (zip içinde `io-shield.stl` bulunur). Sayfa normal bir tarayıcı sekmesinde açıldığında düz indirme çalışmalıdır. Modeli başka yerde yeniden üretmek için **Yerleşimi JSON kopyala** da yedek yol olarak vardır.

### Kod haritası (insanlar ve yapay zekâ ajanları için)

Her şey tek bir HTML dosyasındadır; betiğin başında numaralı bölümler vardır:

| # | Bölüm | İçerik |
|---|---|---|
| 1 | `CONFIG` | Varsayılanlar: plaka ölçüsü, yapışma mesafeleri, dışa aktarma adımı, rampa adımları, fotoğraf boyut sınırı |
| 2 | `PRESETS` | `{ w, h, r }` biçiminde mm cinsinden port ölçüleri. **Yeni konnektör için bir satır eklemek yeterli.** |
| 3 | `STATE` | Tek değişken veri (`state`); arayüz buradan çizilir. `TR` çeviri tablosu ve `t()` de burada |
| 4 | Geometri | `solveHomography`, `toMatrix3d` (fotoğraf → plaka dönüşümü), `holeSpan` / `holesAt` (kesimler), `buildLayers` / `catchRects` (flanş, gövde, geçmeli çıkıntılar), `snapPosition` (yapışma) |
| 5 | Dışa aktarma | `buildStl` (katmanlı dilim modeli), `zipStore` (tek dosyalı ZIP), `buildSvg`, `saveFile` (indirme sarmalayıcısı) |
| 6 | Çizim | `renderAlign`, `renderDesign`, `rimSvg`, `overlaySvg`, `measureSvg`, alan oluşturucular |
| 7 | Olaylar | `bindDrag` (iki görünüm için işaretçi sürükleme), klavye, girişler, düğmeler, `setLang` |
| 8 | Başlatma | İlk çizim |

Kurallar: plaka uzayı milimetredir; başlangıç noktası sol üsttür ve y aşağı doğrudur (kasanın dışından bakıldığında). STL'de `z = 0` flanşın dış yüzüdür ve `z` kasanın içine doğru artar.

**Dil eklemek:** `TR` yanına başka bir tablo ekleyin (anahtarlar İngilizce metinlerdir), `t()` işlevini ve `setLang()` içindeki düğmeyi genişletin. Sabit metinler DOM taranarak otomatik çevrilir; JS ile üretilen metinler `t()` üzerinden geçmelidir.

### Fikirler / henüz yapılmadı

3MF dışa aktarma · dönebilen portlar · plaka köşe yarıçapı · vida kulakları · iki parmakla yakınlaştırma · fotoğraftan otomatik port algılama · projeyi otomatik kaydetme.
