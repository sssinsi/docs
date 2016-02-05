..
   Shell Helpers

シェルヘルパー
#############

..
    Shell Helpers were added in 3.1.0

.. versionadded:: 3.1
    シェルヘルパーが追加されました。

..
    Shell Helpers let you package up complex output generation code. Shell
    Helpers can be accessed and used from any shell or task::

シェルヘルパーで複雑な出力生成コードをパッケージ化しましょう。
シェルヘルパーはあらゆるシェルやタスクから使うことができます。::

    // テーブルデータを出力します
    $this->helper('table')->output($data);

    // プラグインからヘルパーを取得します
    $this->helper('Plugin.HelperName')->output($data);

..
    You can also get instances of helpers and call any public methods on them::

また、ヘルパーのインスタンスを取得したり、ヘルパーのパブリック関数を呼ぶことができます。::

    // Progressヘルパーを取得し、使用します
    $progress = $this->helper('Progress');
    $progress->increment(10);
    $progress->draw();

..
    Creating Helpers

ヘルパーの作成
================

..
    While CakePHP comes with a few shell helpers you can create more in your
    application or plugins. As an example, we'll create a simple helper to generate
    fancy headings. First create the **src/Shell/Helper/HeadingHelper.php** and put
    the following in it::

CakePHPにいくつかのシェルヘルパーが付随していますが、アプリケーションやプラグインの中で作ることができます。
たとえば、派手な見出しの簡単はヘルパーを作ってみましょう。
はじめに、**src/Shell/Helper/HeadingHelper.php**を作成して以下のように書きます。::


    <?php
    namespace App\Shell\Helper;

    use Cake\Console\Helper;

    class HeadingHelper extends Helper
    {
        public function output($args)
        {
            $args += ['', '#', 3];
            $marker = str_repeat($args[1], $args[2]);
            $this->_io->out($marker . ' ' . $args[0] . ' ' . $marker);
        }
    }

..
    We can then use this new helper in one of our shell commands by calling it::

この新しいヘルパーをシェルコマンドで呼び出すことで使用できます。::

    // ###を両側に出力します
    $this->helper('heading')->output('It works!');

    // ~~~~を両側に出力します
    $this->helper('heading')->output('It works!', '~', 4);

..
    Built-In Helpers

ビルトインヘルパー
================

..
    Table Helper

テーブルヘルパー
------------

..
    The TableHelper assists in making well formatted ASCII art tables. Using it is
    pretty simple::

テーブルヘルパーはASCIIアートテーブルの作成を助けます。使い方はとても簡単です。::

        $data = [
            ['Header 1', 'Header', 'Long Header'],
            ['short', 'Longish thing', 'short'],
            ['Longer thing', 'short', 'Longest Value'],
        ];
        $this->helper('table')->output($data);

        // Outputs
        +--------------+---------------+---------------+
        | Header 1     | Header        | Long Header   |
        +--------------+---------------+---------------+
        | short        | Longish thing | short         |
        | Longer thing | short         | Longest Value |
        +--------------+---------------+---------------+

..
    Progress Helper

プログレスヘルパー
---------------

..
    The ProgressHelper can be used in two different ways. The simple mode lets you
    provide a callback that is invoked until the progress is complete::

プログレスヘルパーは２つの異なる使い方ができます。
簡単な方法は、経過が完了するまで起動するコールバックを提供します。::

    $this->helper('progress')->output(function ($progress) {
        // ここで処理
        $progress->increment(20);
    });

..
    You can control the progress bar more by providing additional options:

追加オプションを提供することで、経過バーを制御することができます。:

..  - ``total`` The total number of items in the progress bar. Defaults
..    to 100.
..  - ``width`` The width of the progress bar. Defaults to 80.
..  - ``callback`` The callback that will be called in a loop to advance the
..    progress bar.


- ``total`` 経過バーの総数。初期値は100。
- ``width`` 経過バーの幅。初期値は80。
- ``callback`` ループ内での経過バーを進めるためのコールバック。

..
    An example of all the options in use would be::

例えば、すべてのオプションを使用すると以下のようになります。::

    $this->helper('progress')->output([
        'total' => 10,
        'width' => 20,
        'callback' => function ($progress) {
            $progress->increment(2);
        }
    ]);

..
    The progress helper can also be used manually to increment and re-render the
    progress bar as necessary::

必要に応じてプログレスヘルパーは経過バーを手動で増加、または再描画できます。::

    $progress = $this->helper('Progress');
    $progress->init([
        'total' => 10,
        'width' => 20,
    ]);

    $this->helper->increment(4);
    $this->helper->draw();
