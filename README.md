# Harmonic Pro+ (Strict) - Multi-Scale Auto Scanner

Pine Script v5 indicator that scans multiple pivot lengths to detect harmonic patterns and draws PRZ, pattern legs, and optional trade levels (Entry/TP1/TP2/SL). It also enforces strict filters such as ratio tolerance, time symmetry, alternation, trend filter, quality score, and reversal confirmation.

## Features
- Multi-scale scanning across many rightbars lengths (10..500).
- Pattern detection: Bat, Alt Bat, Butterfly, Crab, Deep Crab, Gartley, Cypher, Shark (113/886).
- Strict filters:
  - Ratio tolerance and pattern scoring.
  - Time symmetry (AB vs BC) and alternation rule.
  - Optional trend filter (SMA).
  - Minimum quality score.
  - D-time constraints and timeout.
  - Reversal confirmation (engulfing / pin bar / close-break).
- PRZ box and pattern lines for predicted (untriggered) and historical (triggered) setups.
- TP/SL lines with labels and RR/risk display.
- Auto cleanup of expired or invalid predicted drawings.

## How It Works (High-Level)
1. Pivots are detected per length and stored in a rolling cache.
2. XA/AB/BC ratios are checked against harmonic templates.
3. A score combines ratio quality, time symmetry, and alternation.
4. PRZ is computed from XD and BD ranges.
5. A trigger occurs if price intersects the PRZ.
6. Optional reversal confirmation is required after trigger.
7. D must be extreme within the C->trigger interval, and price must not breach the opposite PRZ side after trigger.
8. Drawings are created for predictions; on trigger, prediction objects are removed and historical objects are drawn.

## Inputs
Key inputs are grouped in the script:

### Scan Settings
- `scan_min`, `scan_max`, `scan_step`: scan range control (actual scan is expanded manually in the script).

### Strict Filters
- `tol_pct`: ratio tolerance in percent.
- `time_sym_pct`: AB vs BC time symmetry tolerance.
- `time_limit_mult`: D-timeout multiplier (relative to BC).
- `pin_wick_ratio`, `pin_body_max`: pin bar detection thresholds.
- `rev_max_offset`: max bars back for reversal confirmation.
- `alt_ratio`: alternation slope multiplier.
- `use_trend`, `trend_len`: trend filter on SMA.
- `min_score`: minimum quality score (0-100).

### Draw
- `line_future`: projection width for PRZ/levels.
- `show_prz_box`: show PRZ box for predictions.
- `show_score`: show score on label.
- `show_levels`: show Entry/TP/SL levels.

## TP/SL Rules (Current)
- TP base:
  - Cypher and Sharks: CD length.
  - Others: AD length.
  - Shark TPs: 0.5 and 0.886.
  - Others: 0.382 and 0.618.
- SL:
  - Gartley/Cypher: at X.
  - Bat: XA 1.13
  - Alt Bat: XA 1.272
  - Butterfly: XA 1.414
  - Crab/Deep Crab: XA 2.0
  - Shark: XC 0.886 -> SL XC 1.0; XC 1.13 -> SL XC 1.272

## Usage
1. Open TradingView, create a new Pine Script.
2. Paste `main.pine` content.
3. Add to chart and adjust inputs in the settings panel.

## Notes / Limitations
- Multiple run lengths are hardcoded for performance and Pine restrictions.
- Heavy scans can impact chart performance; reduce lengths if needed.
- Predictions are pruned when invalid or expired.
- There are hard limits on drawing objects (lines/labels/boxes).

## Files
- `main.pine`: primary indicator script.
