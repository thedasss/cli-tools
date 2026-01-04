#!/usr/bin/env bash
# get-project-structure (global version with keyword exclude support)
# Usage: get-project-structure [-o output_file] [-e folder_to_exclude] [-k keyword]

set -euo pipefail

PROJECT_ROOT=$(pwd)
OUTPUT_FILE=""
EXTRA_EXCLUDES=()
KEYWORDS=()

# --- Parse arguments ---
while [[ $# -gt 0 ]]; do
    case $1 in
        -o|--output)
            OUTPUT_FILE="$2"
            shift 2
            ;;
        -e|--exclude)
            EXTRA_EXCLUDES+=("$2")
            shift 2
            ;;
        -k|--keyword)
            KEYWORDS+=("$2")
            shift 2
            ;;
        -h|--help)
            echo "Usage: $0 [-o output_file] [-e folder_to_exclude] [-k keyword]"
            exit 0
            ;;
        *)
            echo "❌ Unknown option: $1"
            echo "Usage: $0 [-o output_file] [-e folder_to_exclude] [-k keyword]"
            exit 1
            ;;
    esac
done

# --- Build exclusion list from .gitignore ---
DIR_IGNORES=()
FILE_IGNORES=()

if [ -f "$PROJECT_ROOT/.gitignore" ]; then
    while IFS= read -r line || [[ -n "$line" ]]; do
        # Skip comments and empty lines
        [[ "$line" =~ ^#.*$ || -z "$line" ]] && continue
        # Normalize trailing slashes
        if [[ "$line" == */ ]]; then
            DIR_IGNORES+=("${line%/}")
        else
            FILE_IGNORES+=("$line")
        fi
    done < "$PROJECT_ROOT/.gitignore"
fi

# Add extra excludes
for ex in "${EXTRA_EXCLUDES[@]}"; do
    DIR_IGNORES+=("$ex")
done

# --- Build keyword regex ---
KEYWORD_REGEX=""
for kw in "${KEYWORDS[@]}"; do
    safe_kw=$(printf "%s" "$kw" | sed 's/[.[\^$+?(){}|]/\\&/g')
    if [[ -z "$KEYWORD_REGEX" ]]; then
        KEYWORD_REGEX="$safe_kw"
    else
        KEYWORD_REGEX="$KEYWORD_REGEX|$safe_kw"
    fi
done

# --- Build tree/find command ---
if command -v tree &>/dev/null; then
    CMD=(tree -a)
    # exclude dirs/files from tree
    if [[ ${#DIR_IGNORES[@]} -gt 0 || ${#FILE_IGNORES[@]} -gt 0 ]]; then
        PATTERN=$(IFS="|"; echo "${DIR_IGNORES[*]}|${FILE_IGNORES[*]}")
        CMD+=("-I" "$PATTERN")
    fi
else
    CMD=(find .)
fi

# --- Run command ---
if [[ -n "$OUTPUT_FILE" ]]; then
    if [[ -n "$KEYWORD_REGEX" ]]; then
        "${CMD[@]}" | grep -Ev "$KEYWORD_REGEX" > "$OUTPUT_FILE"
    else
        "${CMD[@]}" > "$OUTPUT_FILE"
    fi
    echo "✅ Project structure saved to $OUTPUT_FILE"
else
    if [[ -n "$KEYWORD_REGEX" ]]; then
        "${CMD[@]}" | grep -Ev "$KEYWORD_REGEX"
    else
        "${CMD[@]}"
    fi
fi
