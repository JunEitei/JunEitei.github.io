| コード      | 言語名             |                    |
|-------------|-------------------|--------------------|
| .ja         | 日本語             | 🇯🇵                |
| .de         | Deutsch            | 🇩🇪                |
| .en         | English            | 🇺🇸                |
| .ar         | العربية             | 🇸🇦                |
| .fr         | Français           | 🇫🇷                |
| .hi         | हिन्दी             | 🇮🇳                |
| .bn         | বাংলা              | 🇧🇩                |
| .ru         | Русский            | 🇷🇺                |
| .ko         | 한국어             | 🇰🇷                |
| .es         | Español            | 🇪🇸                |
| .pt         | Português          | 🇵🇹                |
| .it         | Italiano           | 🇮🇹                |
| .ms         | Bahasa Melayu      | 🇲🇾                |
| .fi         | Suomi              | 🇫🇮                |
| .sw         | Kiswahili          | 🇰🇪                |
| .th         | ภาษาไทย            | 🇹🇭                |
| .el         | Ελληνικά           | 🇬🇷                |
| .hu         | Magyar             | 🇭🇺                |
| .tr         | Türkçe            | 🇹🇷                |
| .vi         | Tiếng Việt         | 🇻🇳                |
| .zh      | 繁體中文           |                    |
| .he        | עברית             | 🇮🇱                |
| .fa         | فارسی              | 🇮🇷                |
| .cs         | Čeština           | 🇨🇿                |
| .uk         | Українська        | 🇺🇦                |
| .id         | Indonesian         | 🇮🇩                |
| .da         | Dansk              | 🇩🇰                |
| .no         | Norsk              | 🇳🇴                |
| .pl         | Polski             | 🇵🇱                |
| .nl         | Nederlands         | 🇳🇱                |
| .ro         | Română             | 🇷🇴                |
| .sv         | Svenska            | 🇸🇪                |

# Python スクリプト

以下は、Markdown ファイルをスウェーデン語に翻訳する Python スクリプトやわ。

```python
import os
from deep_translator import GoogleTranslator, exceptions

# 入力と出力のフォルダパスを定義
input_folder = 'Posts'  # 入力の Markdown フォルダ
output_folder = 'Posts_sv'  # 出力の Markdown フォルダ、スウェーデン語に指定

# 出力フォルダを作成（存在しない場合）
os.makedirs(output_folder, exist_ok=True)

# 入力フォルダ内のすべての Markdown ファイルをループ処理
for filename in os.listdir(input_folder):
    if filename.endswith('.md'):
        input_file_path = os.path.join(input_folder, filename)  # 入力ファイルのパス

        # 出力ファイル名を変更し、拡張子を .ja.md から .sv.md に置き換え
        output_file_name = filename.replace('.ja.md', '.sv.md')  # 拡張子を置き換え
        output_file_path = os.path.join(output_folder, output_file_name)  # 出力ファイルのパス

        # Markdown ファイルの内容を読み取る
        with open(input_file_path, 'r', encoding='utf-8') as file:
            content = file.readlines()  # ファイル内容を行ごとに読み取る

        # 最初の4行を抽出
        front_matter = content[:4]  # 最初の4行を保持
        translated_lines = []

        # 残りの内容を翻訳
        for line in content[4:]:  # 5行目から翻訳を開始
            trimmed_line = line.rstrip()  # 行末の空白を取り除く

            # 元の行の改行コードと末尾の空白を記録
            newline_character = '\n' if line.endswith('\n') else ''
            trailing_spaces = len(line) - len(trimmed_line)  # 末尾の空白の数を計算
            try:
                translated_line = GoogleTranslator(source='auto', target='swedish').translate(trimmed_line)
                if translated_line is None:
                    translated_line = trimmed_line  # 翻訳が None の場合は元の行を使用

            except exceptions.NotValidPayload:  # 無効なペイロードエラーを処理
                translated_line = trimmed_line  # 翻訳エラーが発生した場合は元の行を使用

            # 元の空白と改行コードを追加
            translated_lines.append(translated_line + ' ' * trailing_spaces + newline_character)

        # 翻訳された内容を結合
        final_content = front_matter + translated_lines

        # タイトル行を更新
        for i, line in enumerate(final_content):
            if line.startswith('title ='):
                # 元のタイトルを抽出し、更新
                original_title = line.split('"')[1]  # 引用符内の元のタイトルを抽出
                new_title = GoogleTranslator(source='auto', target='swedish').translate(original_title)  # タイトルを翻訳
                final_content[i] = f'title = "{new_title}"\n'  # 新しいタイトルに置き換え

        # 翻訳された内容を新しいファイルに保存
        with open(output_file_path, 'w', encoding='utf-8') as file:
            file.writelines(final_content)  # 翻訳されたすべての行を書き込む

        print(f"翻訳完了: {filename} -> {output_file_path}")

print("すべてのファイルの翻訳が完了しました！")
