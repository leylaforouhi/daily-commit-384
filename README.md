
from pathlib import Path
from collections import defaultdict

AUDIO_FORMATS = {
    ".mp3", ".wav", ".flac",
    ".aac", ".ogg", ".m4a", ".wma"
}

def analyze_audio(folder):
    folder = Path(folder)

    if not folder.is_dir():
        print("Invalid folder path.")
        return

    formats = defaultdict(lambda: {
        "count": 0,
        "size": 0
    })

    for file in folder.rglob("*"):
        if file.is_file() and file.suffix.lower() in AUDIO_FORMATS:
            extension = file.suffix.lower()
            formats[extension]["count"] += 1
            formats[extension]["size"] += file.stat().st_size

    if not formats:
        print("No audio files found.")
        return

    print("\n--- Audio File Report ---")

    for extension, data in sorted(formats.items()):
        size_mb = data["size"] / (1024 * 1024)

        print(f"\nFormat: {extension}")
        print(f"Files: {data['count']}")
        print(f"Total size: {size_mb:.2f} MB")

    total_files = sum(item["count"] for item in formats.values())
    total_size = sum(item["size"] for item in formats.values())

    print("\n-------------------------")
    print(f"Total audio files: {total_files}")
    print(f"Total size: {total_size / (1024 * 1024):.2f} MB")

def main():
    folder = input("Enter folder path: ").strip()
    analyze_audio(folder)

if __name__ == "__main__":
    main()
