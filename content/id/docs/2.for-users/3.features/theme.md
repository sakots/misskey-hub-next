# Tema

Kamu bisa mengubah tampilan klien Misskey dengan mengatur temanya.

## Pengaturan Tema

[Pengaturan > Tema](x-mi-web://settings/theme)

## Membuat Tema

Kode objek tema ditulis menggunakan JSON5.
Tema memiliki tibe objek seperti yang ditunjukkan di bawah.

```js
{
	id: '17587283-dd92-4a2c-a22c-be0637c9e22a',

	name: 'Danboard',
	author: 'syuilo',

	base: 'light',

	props: {
		accent: 'rgb(218, 141, 49)',
		bg: 'rgb(218, 212, 190)',
		fg: 'rgb(115, 108, 92)',
		panel: 'rgb(236, 232, 220)',
		renote: 'rgb(100, 152, 106)',
		link: 'rgb(100, 152, 106)',
		mention: '@accent',
		hashtag: 'rgb(100, 152, 106)',
		header: 'rgba(239, 227, 213, 0.75)',
		navBg: 'rgb(216, 206, 182)',
		inputBorder: 'rgba(0, 0, 0, 0.1)',
	},
}

```

- `id` ... テーマの一意なID。UUIDをおすすめします。
- `name` ... テーマ名
- `author` ... テーマの作者
- `desc` ... テーマの説明(オプション)
- `base` ... 明るいテーマか、暗いテーマか
  - `light`にすると明るいテーマになり、`dark`にすると暗いテーマになります。
  - テーマはここで設定されたベーステーマを継承します。
- `props` ... テーマのスタイル定義。これから説明します。

### Definisi Gaya Tema

Definisikan gaya tema di dalam `props`.
Kunci merupakan nama dari variabel, dan nilai menentukan konten.
Selanjutnya, objek `props` ini mewariskan dari tema dasar.
Tema dasarnya adalah [\_light.json5][_light.json5] jika `base` dari tema ini adalah `light` dan [\_dark.json5][_dark.json5] jika `dark`.
Artinya, jika tidak ada kunci `props` yang bernama `panel` dalam tema ini, maka nilai `panel` akan diatur menggunakan nilai dari tema dasar.

- [_light.json5]: https://github.com/misskey-dev/misskey/blob/develop/packages/frontend/src/themes/_light.json5
- [_dark.json5]: https://github.com/misskey-dev/misskey/blob/develop/packages/frontend/src/themes/_dark.json5

#### バリューで使える構文

- 16進数で表された色
  - 例: `#00ff00`
- `rgb(r, g, b)`形式で表された色
  - 例: `rgb(0, 255, 0)`
- `rgba(r, g, b, a)`形式で表された透明度を含む色
  - 例: `rgba(0, 255, 0, 0.5)`
- 他のキーの値の参照
  - `@{キー名}`と書くと他のキーの値の参照になります。`{キー名}`は参照したいキーの名前に置き換えます。
  - 例: `@panel`
- 定数(後述)の参照
  - `${定数名}`と書くと定数の参照になります。`{定数名}`は参照したい定数の名前に置き換えます。
  - 例: `$main`
- 関数(後述)
  - `:{関数名}<{引数}<{色}`

#### 定数

「CSS変数として出力はしたくないが、他のCSS変数の値として使いまわしたい」値があるときは、定数を使うと便利です。
キー名を`$`で始めると、そのキーはCSS変数として出力されません。

#### 関数

「ボタンの上にカーソルを合わせたときだけ色を明るくしたい」のように、既存の色から少し変更した色を使いたい場合に、関数を使うと便利です。

`:{関数名}<{引数}<{色や他のキーの参照}`の形で使うことができます。

```js
props: {
	accent: '#86b300',
	accentDarken: ':darken<10<#86b300',
	accentLighten: ':lighten<10<@accent'
}
```

##### 使用できる関数

- `lighten` ... 渡された色の輝度(0 ~ 100)に対して引数(0 ~ 100)を加算した色を返します。
- `darken` ... 渡された色の輝度(0 ~ 100)に対して引数(0 ~ 100)を減算した色を返します。
- `alpha` ... 渡された色の透明度を引数(0.0 ~ 1.0)に設定した色を返します。
  - 0.0のとき完全に透明、1.0で完全に不透明になります。
- `hue` ... 渡された色の色相(-360 ~ 360)に対して引数(-360 ~ 360)の値だけ回転させた色を返します。
- `saturate` ... 渡された色の彩度(0 ~ 100)に対して引数(0 ~ 100)を加算した色を返します。

## テーマを配布する

v2023.11.0以降では、あなたのウェブサイトから、ワンクリックでテーマを直接インストールできるようになっています。

テーマのインストール機能を提供する場合は、あなたのサイト上にAPIを実装する必要があります。詳しくは[こちら](../../for-developers/publish-on-your-website/)をご覧ください。
