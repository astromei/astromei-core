# Astromei Core · 六曜計算ライブラリ

The independent calculation library behind [Astromei’s Rokuyo calendar tools](https://astromei.com/ja/rokuyo). Local computation, ESM and TypeScript declarations, Node.js 20+, MIT licensed.

[Astromei](https://astromei.com) · [About](https://astromei.com/ja/about) · [MCP adapter](https://github.com/astromei/rokuyo-mcp)

## 日本語

Astromei（アストロメイ）の六曜計算コアです。日付の六曜・旧暦月日・読み方・時間帯の慣習、月間カレンダー、大安などの日付検索を提供します。APIキー不要でローカル動作します。対応期間は **2025年1月1日〜2027年12月31日**。入力は日本時間の暦日です。

この初版は六曜を対象としています。四柱推命・紫微斗数・西洋占星術のエンジンは含みません。AI・MCP・Web UI に依存しないため、別のアプリから直接利用できます。

## Installation

**npm registry publication is pending.** Use the GitHub source now:

```sh
git clone https://github.com/astromei/astromei-core.git
cd astromei-core
npm ci
npm test
```

After the npm release:

```sh
npm install astromei-core
```

```js
import { getRokuyo, getRokuyoMonth, findRokuyoDates } from 'astromei-core';

getRokuyo('2026-10-11'); // 先負, lunar month 9 / day 1
getRokuyoMonth(2026, 11);
findRokuyoDates('2026-11-01', '2026-11-30', '大安');
```

From a source checkout, import from `./dist/index.js` instead.

## API

| Export | Behavior |
|---|---|
| `getRokuyo(date)` | Rokuyo, lunar date, Japanese reading, romaji, description and morning/noon/afternoon customs |
| `getRokuyoMonth(year, month)` | All Gregorian dates in the requested month |
| `findRokuyoDates(start, end, rokuyo = '大安')` | Inclusive date search, capped at 366 days |
| `ROKUYO` / `SUPPORTED_RANGE` | Six labels and supported date bounds |
| `CALENDAR_METHOD` / `CULTURAL_NOTE` | Calculation method and cultural context |
| `Rokuyo` / `RokuyoDay` | TypeScript types |

Invalid dates, unsupported years, labels or ranges throw `RangeError`. Dates use `YYYY-MM-DD` and `Asia/Tokyo` calendar-day semantics; no current clock or network is used.

## Calendar method and limitations


Rokuyo is calculated from the lunar month and day: `['先勝','友引','先負','仏滅','大安','赤口'][(month + day - 2) % 6]`. Leap months use the corresponding month number.

Lunar conversion uses the MIT-licensed [`korean-lunar-calendar`](https://github.com/usingsky/korean_lunar_calendar_js), based on KASI's UTC+9 calendar. This matches the convention used by Astromei's Japanese calendar. It is **not an official Japanese calendar**, and Korean/Japanese calendar conventions should not be assumed identical in all historical or future years. This initial release is deliberately restricted to the 2025–2027 window previously checked for Astromei. Different calendar publications may use different conventions.

The Japan/China midnight new-moon boundary matters: in Astromei's convention, **2026-10-10 = lunar 8/30** and **2026-10-11 = lunar 9/1 (先負)**. Substituting a UTC+8 Chinese lunar date shifts the Rokuyo labels for that month.

Time-slot descriptions are cultural conventions and are simplified; they are not forecasts or professional advice. This package does not calculate other almanac annotations, holidays, astronomy or birth charts.

## Development

```sh
npm ci
npm test
npm pack
```

Tests cover the Japan midnight new-moon boundary, known dates, month lengths, invalid inputs, bounded searches and independent result objects. The [MCP repository](https://github.com/astromei/rokuyo-mcp) handles protocol integration separately.

MIT licensed. Maintained by [Astromei](https://astromei.com).
