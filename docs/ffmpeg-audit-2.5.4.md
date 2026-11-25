# Replay 2.5.4 i FFmpeg na Windows x64

## Kompilacja w instalatorze 2.5.3

Oba pliki z katalogu `tools/ffmpeg` mają wersję `N-126374-g089a48eb36-20260831`
i pasują bajt w bajt do statycznego wariantu Windows x64 wydania
`autobuild-2026-08-31-20-15`. Archiwum binarne tego wariantu ma SHA-256
`fc496061ed2cc5264d7c9c4ec929365f15267aeb40c94fd34ce637a0ea6ce229`.

| Plik | SHA-256 |
| --- | --- |
| ffmpeg.exe | b9989f24e63be74e72aa45fec10a114ce660b562e4303d5f296f4fc8ba7cc896 |
| ffprobe.exe | 73842bee6f270e85b2ab6fc3cfa715f614f16ad16700f5de6a9c1a2865cacf00 |
| Replay-Setup.exe | 914d2a4fb6b38fad9cd661cd0e1c02db358eb98692535a74bb555999e577102f |

Flagi `--enable-gpl --enable-version3` i wynik `-L` identyfikują
licencję GNU GPL 3 lub nowszą. Upstream użył GCC 15.2.0 i crosstool-NG
1.28.0.23_185f348. Jego skrypt kompilacji używa `--pkg-config-flags=--static`.

- [Wydanie binarne yt-dlp](https://github.com/yt-dlp/FFmpeg-Builds/releases/tag/autobuild-2026-08-31-20-15)
- [Kod źródłowy FFmpeg z dokładnego commita](https://github.com/FFmpeg/FFmpeg/tree/089a48eb36b2c0d185867522a1fc2613b5bbb96f)
- [Skrypty, poprawki i definicje zależności użyte przez yt-dlp](https://github.com/yt-dlp/FFmpeg-Builds/tree/ea2ec3c0e0dfb11069729b7df5cb234bb2145956)
- [Oryginalny przebieg budowania](https://github.com/yt-dlp/FFmpeg-Builds/actions/runs/33431833698)

Odzyskano 90 archiwów wejściowych widocznych w logu wariantu win64 GPL.
W katalogu poprawek FFmpeg był tylko plik utrzymujący katalog w Git.
Budowanie bibliotek zawierało osobne poprawki AOM i aribb24.
Skrypt rav1e aktualizował zależność `cc` w trakcie budowania. Archiwum wejściowe
zawiera drzewa źródeł sprzed tego kroku. Brak potwierdzenia końcowego zestawu
zależności rozwiązywanych przez Cargo jest podstawą wymiany tych plików.
Zachowana kopia wejść nie jest kompletnym pakietem odpowiadających źródeł
dla historycznych instalatorów.

## Kompilacja dla 2.5.4

Wersja `9.1-Replay.1`, statyczny Windows x64, GNU GPL 3 lub nowsza. Źródła
FFmpeg 9.1 oraz x264, LAME, Intel VPL, nagłówki NVIDIA i AMD i źródła
odpowiednich bibliotek narzędziowych są w archiwum razem z konfiguracją i
skryptem budowania. FFmpeg nie wymaga skryptu yt-dlp z poprzedniego wydania.

| Plik | SHA-256 |
| --- | --- |
| ffmpeg.exe | c0c66f6b1483e986fa64e6e5ef2563deb8b9a65ce9872d8557a5232a3eeead72 |
| ffprobe.exe | 5c55e67cdf1e08db8c7857e5b34d2d7dae478304b32e3b7037580d01e6b4fb82 |
| ffmpeg-9.1-Replay.1-win64-corresponding-source.tar.gz | 46640c8c29ee3f07b4e9161a09157cc8357ed79085f4f2725bacb682be3adb7f |

- [Oficjalne źródło FFmpeg 9.1](https://ffmpeg.org/releases/ffmpeg-9.1.tar.xz)
- [Kompilacja i pakiet pełnych źródeł](https://github.com/Aropatx/Replay/actions/runs/45687311412#artifacts)

Replay uruchamia `ffmpeg.exe` i `ffprobe.exe` jako osobne procesy z
argumentami interfejsu wiersza polecenia. Wymienia pliki multimedialne, JSON
metadanych oraz sygnał audio o udokumentowanym formacie. Nagrywanie,
buforowanie i zapis MP4 obsługuje osobny silnik Rust przez Windows Graphics
Capture lub DXGI i Media Foundation. Odtwarzacz WPF korzysta z Windows Media
Foundation. Te granice przemawiają za traktowaniem instalatora jako zbioru
oddzielnych programów według sekcji 5 i 6 GNU GPL. Odpowiadające źródła
dystrybuowanych narzędzi FFmpeg należy oferować obok instalatora.

[GNU GPL v3](https://www.gnu.org/licenses/gpl-3.0.html)

Instalator dołącza również licencje i informacje o .NET 8.0.31, WPF, Windows
Forms, pakietach silnika Rust, bibliotece standardowej Rust 1.99.1,
Inno Setup 7.1.1 i Pascal Script. LZMA SDK jest wymieniony w informacjach
o instalatorze. Silnik statycznie używa środowiska C/C++ Microsoft i
korzysta z bibliotek systemowych Windows.
