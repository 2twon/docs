---
title: プルリクエスト内の依存関係の変更をレビューする
intro: 'プルリクエストに依存関係への変更が含まれている場合は、変更内容の概要と、依存関係に既知の脆弱性があるかどうかを確認できます。'
versions:
  free-pro-team: '*'
---

{% note %}

**注釈:** 依存関係のレビューは現在ベータであり、変更される可能性があります。

{% endnote %}

### 依存関係のレビューについて

プルリクエストがリポジトリのデフォルトブランチを対象とし、パッケージマニフェストまたはロックファイルへの変更が含まれている場合は、依存関係のレビューを表示して、何が変更されたかを確認できます。 依存関係のレビューには、ロックファイル内の間接的な依存関係への変更の詳細が含まれ、追加または更新された依存関係のいずれかに既知の脆弱性が含まれているかどうかが示されます。

依存関係のレビューは次の項目で確認できます。

* すべてのパブリックリポジトリ
* 依存関係グラフが有効になっている {% data variables.product.prodname_advanced_security %} ライセンスを持つ Organization が所有するプライベートリポジトリ。 詳しい情報については、「[リポジトリの依存関係を調べる](/github/visualizing-repository-data-with-graphs/exploring-the-dependencies-of-a-repository#enabling-and-disabling-the-dependency-graph-for-a-private-repository)」を参照してください。

時に、マニフェスト内の 1 つの依存関係のバージョンを更新して、プルリクエストを生成することがあります。 ただし、この直接依存関係の更新バージョンでも依存関係が更新されている場合は、プルリクエストに予想よりも多くの変更が加えられている可能性があります。 各マニフェストとロックファイルの依存関係のレビューにより、何が変更されたか、新しい依存関係バージョンのいずれかに既知の脆弱性が含まれているかどうかを簡単に確認できます。

プルリクエストで依存関係のレビューを確認し、脆弱性としてフラグが付けられている依存関係を変更することで、プロジェクトに脆弱性が追加されるのを防ぐことができます。 {% data variables.product.prodname_dependabot_alerts %} は、すでに依存関係にある脆弱性を検出しますが、あとで修正するよりも、潜在的な問題の発生を回避した方がはるかに良いです。 {% data variables.product.prodname_dependabot_alerts %} に関する詳しい情報については、「[脆弱性のある依存関係に対するアラートについて](/github/managing-security-vulnerabilities/about-alerts-for-vulnerable-dependencies#dependabot-alerts-for-vulnerable-dependencies)」を参照してください。

依存関係のレビューは、依存関係グラフと同じ言語とパッケージ管理エコシステムをサポートしています。 詳しい情報については、「[依存関係グラフについて](/github/visualizing-repository-data-with-graphs/about-the-dependency-graph#supported-package-ecosystems)」を参照してください。

### プルリクエスト内の依存関係を確認する

{% data reusables.repositories.sidebar-pr %}
{% data reusables.repositories.choose-pr-review %}
{% data reusables.repositories.changed-files %}

1. プルリクエストに多数のファイルが含まれている場合は、[**File filter**] ドロップダウンメニューを使用して、依存関係を記録しないすべてのファイルを折りたたみます。 これにより、レビューを依存関係の変更に焦点を絞りやすくなります。

   ![ファイルフィルタメニュー](/assets/images/help/pull_requests/file-filter-menu-json.png)

1. マニフェストまたはロックファイルのヘッダの右側で、リッチ diff ボタンをクリックして依存関係のレビューを表示します。

   ![リッチ diff ボタン](/assets/images/help/pull_requests/dependency-review-rich-diff.png)

  {% note %}

   **注釈:** 依存関係のレビューでは、ソース diff がデフォルトでレンダリングされない大きなロックファイルで何が変更されたかをより明確に確認できます。

   {% endnote %}

1. 依存関係のレビューにリストされている依存関係を確認します。

   ![依存関係のレビューにおける脆弱性の警告](/assets/images/help/pull_requests/dependency-review-vulnerability.png)

   脆弱性のある追加または変更された依存関係が最初に一覧表示され、次に重要度、依存関係名の順に並べられます。 これは、最も重要度の高い依存関係が、常に依存関係レビューの最上位に表示されるということです。 その他の依存関係は、依存関係名のアルファベット順に一覧表示されます。

   各依存関係の横にあるアイコンは、このプルリクエストで依存関係が追加された (<span style="color:#22863a">{% octicon "diff-added" aria-label="Dependency added icon" %}</span>)、更新された (<span style="color:#b08800">{% octicon "diff-modified" aria-label="Dependency modified icon" %}</span>)、削除された (<span style="color:#cb2431">{% octicon "diff-removed" aria-label="Dependency removed icon" %}</span>) ことを示しています。

   その他の情報は次のとおりです。

   * 新規、更新、または削除された依存関係のバージョンまたはバージョン範囲。
   * 依存関係の特定のバージョンの場合:
      * 依存関係のリリース時期。
      * このソフトウェアに依存しているプロジェクトの数。 この情報は、依存関係グラフから取得されます。 依存関係の数を確認すると、誤って間違った依存関係を追加することを防ぐことができます。
      * この依存関係で使用されるライセンス（この情報が利用可能な場合）。 これは、プロジェクトで特定のライセンスが使用されているコードを避ける必要がある場合に役立ちます。

   依存関係に既知の脆弱性がある場合、警告メッセージには次のものが含まれます。

   * 脆弱性の簡単な説明。
   * Common Vulnerabilities and Exposures (CVE) または {% data variables.product.prodname_security_advisories %} (GHSA) 識別番号。 この ID をクリックすると、脆弱性の詳細を確認できます。
   * 脆弱性の重要度。
   * 脆弱性が修正された依存関係のバージョン。 誰かのプルリクエストを確認している場合は、パッチを適用したバージョンまたはそれ以降のリリースに依存関係を更新するようにコントリビューターに依頼することができます。

1. ソースの diff ボタンをクリックすると、ファイルの元のビューに戻ることができます。

   ![ソース diff ボタン](/assets/images/help/pull_requests/dependency-review-source-diff.png)