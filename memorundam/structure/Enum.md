~~~plantuml
@startuml Enum.png
title <size:18> Enum </size>

enum Color {
    BLACK, WHITE, RED, GREEN
}
enum SquareCondition {
    EMPTY, PLACABLE, OCCUPIED, HOLE
}
enum GameStep {
    ORDER_DECIDE, // 手番決め
    PLAY, // 試合
    SCORE_ACCOUNTING, // 勝敗計算
    RESULT // 試合結果
}
enum MoveStep {
    MOVE_REPLACE, // 手番交代、陣営/差し人決定
    BOARD_UPDATE, // 盤面更新
    DISC_SELECT, // 石選択
    SQUARE_SELECT, // 配置マス選択
    TURN_OVER, // ひっくり返る
    EFFECT, // ダメージを受けるなどのエフェクト
    MOVE_COMPLETION // 手番終了
}
enum DiscDeckSet {
    ALL_ONE, // 全て1 (= ただのリバーシ)
    ONE_TO_32, // 1〜32を1つずつ
    ZERO_TO_31, // 0〜31を1つずつ
    MINUS16_TO_16, // -16〜16を1つずつ (0を除く)
    ALL_MINUS_ONE, // 全て-1 (= 負けるが勝ちリバーシ)
    ONLY_ONE_IN_ZEROS, // たったひとつの冴えたやりかた(←は?)
    RANDOM_IN_COMMON, // ランダム生成 (全陣営同一)
    CUSTOM_IN_COMMON, // カスタム (全陣営同一)
    CUSTOM_INDIVIDUALLY // カスタム (各陣営個別)
}
enum DiscDeckRegulation {
    ZERO, // ゼロ使用可能
    NEGA, // 負の数使用可能
    MAX, // 最大値を設定
    MIN, // 最小値を設定
    DUPLICATIVE, // 値の重複可能
    FORMULA // 数式使用可能
}
enum WinningCond {
    // 勝利条件
    MAJORITY, // 石の数が多い方
    HIGHEST_SCORE, // ポイント合計の大きい方
    DEATH_MATCH, // 相手を続行不能にした方
}
enum TurnOverCond {
    // ひっくり返る条件
    PINCER, // 挟んだら
    HP // 体力を削りきったら
}
enum StageBoard {
    QUAD_8X8,
    QUAD_10X10,
    CRACK_8X8,
    CRUMBLING_8X8,
    RANDOM,
    CUSTOM
}

class DeckDesc <struct> {
}
class StageBoardDesc <struct> {
    Array2D<SquareCondition>
}
class GameConfig <struct> {
    vector<PlayerInGame> players
    DeckDesc deckDesc
    WinningCond winningCond
    TurnOverCond turnOverCond
    StageBoardDesc stage
}
@enduml
~~~

/'
enum Color {
    BLACK, WHITE, RED, GREEN
};
enum SquareCondition {
    EMPTY, PLACABLE, OCCUPIED, HOLE
};
enum GameStep {
    /// 手番決め
    ORDER_DECIDE,
    /// 試合
    PLAY,
    /// 勝敗計算
    SCORE_ACCOUNTING,
    RESULT
};
enum MoveStep {
    /// 手番交代、陣営/差し人決定
    MOVE_REPLACE,
    /// 盤面更新
    BOARD_UPDATE,
    /// 石選択
    DISC_SELECT,
    /// 配置マス選択
    SQUARE_SELECT,
    /// ひっくり返る
    TURN_OVER,
    /// ダメージを受けるなどのエフェクト
    EFFECT,
    MOVE_COMPLETION // 手番終了
};
enum DiscDeckSet {
    /// 全て1 (= ただのリバーシ)
    ALL_ONE,
    /// 1〜32を1つずつ
    ONE_TO_32,
    /// 0〜31を1つずつ
    ZERO_TO_31,
    /// -16〜16を1つずつ (0を除く)
    MINUS16_TO_16,
    /// 全て-1 (= 負けるが勝ちリバーシ)
    ALL_MINUS_ONE,
    /// たったひとつの冴えたやりかた(←は?)
    ONLY_ONE_IN_ZEROS,
    /// ランダム生成 (全陣営同一)
    RANDOM_IN_COMMON,
    /// カスタム (全陣営同一)
    CUSTOM_IN_COMMON,
    CUSTOM_INDIVIDUALLY // カスタム (各陣営個別)
};
enum DiscDeckRegulation {
    /// ゼロ使用可能
    ZERO,
    /// 負の数使用可能
    NEGA,
    /// 最大値を設定
    MAX,
    /// 最小値を設定
    MIN,
    /// 値の重複可能
    DUPLICATIVE,
    FORMULA // 数式使用可能
};
enum WinningCond {
    // 勝利条件
    /// 石の数が多い方
    MAJORITY,
    /// ポイント合計の大きい方
    HIGHEST_SCORE,
    /// 相手を続行不能にした方
    DEATH_MATCH,
};
enum TurnOverCond {
    // ひっくり返る条件
    /// 挟んだら
    PINCER,
    HP // 体力を削りきったら
};
enum StageBoard {
    QUAD_8X8,
    QUAD_10X10,
    CRACK_8X8,
    CRUMBLING_8X8,
    RANDOM,
    CUSTOM
};
'/
