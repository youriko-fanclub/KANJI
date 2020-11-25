# BattleScene

## todo
- [ ] 漢字
    - [ ] 物理的実体
        - [ ] mgr
    - [ ] 名目的存在
        - [ ] holder
    - [ ] param
        - [ ] static
        - [ ] dynamic
    - [ ] 部首
        - [ ] mgr
    - [ ] 技
        - [ ] mgr
        - [ ] 通常
        - [ ] 必殺
            - [ ] エフェクト
- [ ] timer
    - [ ] 開始
    - [ ] 終了
    - [ ] 演出
    - [ ] pause/resume
- [ ] sequence
    - [ ] pause
    - [ ] resume
    - [ ] no contest
    - [ ] in from stage select
    - [ ] out to result
- [ ] 図鑑
    - [ ] parmanent
    - [ ] temporal
        - [ ] 黒板
- [ ] ui

## uml

```plantuml
@startuml

skinparam backgroundColor pink
skinparam handwritten true

package battle {

class BattleDesc <struct> {
    std::vector<BattlePlayerDesc> m_players
}
class BattlePlayerDesc <struct> {
    std::vector<ParameterizedKanji> m_kanjis
}

BattleDesc o-- BattlePlayerDesc

class BattleScene {
    -BattleManager m_battle_manager
    -BattleUIManager m_battle_ui_manager
    +void initialize()
    +void update()
    +void draw() const
}

interface IBattleManager {
    +bool hasGameSet() const
    +BattleResultDesc createResultDesc() const
    +BattlePlayerManager playerMgr() const
    +PhysicalWorldManager worldMgr() const
    +PhysicalMoveManager moveMgr() const

    +void initialize(BattleDesc desc)
    +void update()
    +void pause()
    +void resume()
    +void holdUp()

    -void lose(PlayerId pid)

}
class BattleManager {
    -BattleTimer m_timer
    -BattlePlayerManager m_player_mgr
    -PhysicalWorldManager m_world_mgr
    -PhysicalMoveManager m_move_mgr
    -BattleResultDesc m_result_desc
    -bool m_has_gameset
}
BattleManager .up.|> IBattleManager
class BattleTimer

class BattlePlayerManager {
    +unordered_map<PlayerId, BattlePlayer> players() const
    +unordered_map<PlayerId, BattlePlayer> alivePlayers() const
    -void lose(PlayerId pid)

    -unordered_map<PlayerId, BattlePlayer> m_players
    -unordered_map<PlayerId, BattlePlayer> m_alivePlayers
}
class BattlePlayer {
    +PlayerId pid() const

    +vector<chara::IParameterizedCharacter> characters() const
    +chara::IParameterizedCharacter activeCharacter()
    +bool changeActiveCharacter()

    +bool hasRadical() const
    +const Radical& radical() const
    +void setRadical(const Radical& value)

    +PhysicalCharacter physical()
    +void initializePhysical(PhysicalCharacter physical)

    +bool isLost() const
    +void addGaveDamage(int amount)
    +void attack(const MomentaryMove& move, bool is_from_left) // 被弾時の処理

    +Score scoreForRanked()
    +void addKOCount()

    -PlayerId m_pid
    -int m_active_index // 3つのうち今どれが場に出ているか
    -vector<chara::IParameterizedCharacter> m_characters // 3つの漢字
    -Radical m_radical
    -PhysicalCharacter m_physical // 物理実体への参照
    -bool m_is_lost
    -Score m_score // 勝敗決定用
}

class PhysicalWorldManager {
    +void initializeCharacters(unordered_map<PlayerId, BattlePlayer> players)
    +void update()

    +unordered_map<PlayerId, PhysicalCharacter> characters() const // これが実体
    +PhysicalStage stage() const
    +void lose(PlayerId pid) // 対応するPhysicalCharacterを退場させる

    -param::CharaPhysics m_param
    -P2World m_world
    -unordered_map<PlayerId, PhysicalCharacter> m_characters
    -PhysicalStage m_stage
}

class PhysicalCharacter {
    +bool isRight() const
    +battle::BattlePlayer* status() const // 参照
    +Vec2 position() const
    +Quad rect() const
    +void shoot(Circular force) // 技が当たった際の吹っ飛ばし。極座標指定

    +void update()

    -PlayerId m_pid
    -P2Body m_body
    -bool m_is_right // 右向きか否か
    -battle::BattlePlayer* m_status
    -param::CharaPhysics m_param
}

class PhysicalMoveManager {
    +PhysicalMove* createMove(PlayerId owner, PhysicalCharacter ownerChara, MoveId moveId)
    +vector<PhysicalMove> moves() const
    +void update(float dt) // 技の更新はここで一括で行う

    -vector<PhysicalMove> m_moves
}

class PhysicalMove {
    PlayerId owner() const // 発動した人
    MomentaryMove currentMoment() const // 今の瞬間の状態を返す
    RectF currentHitBox() const // 今の瞬間の当たり判定
    bool isToRight() const

    bool update(float dt) // 終了したらtrueを返す

    float m_timer // update()のたびに累積
    PlayerId m_owner
    bool m_is_to_right
    PhysicalCharacter m_owner_chara // 技発動しながらキャラが移動するとそれに追従する
    Move m_md // 技の詳細はmasterdataとして登録
}

class PhysicalStage {
    +vector<Vec2> initialCharaPositions(int player_num) const // プレイヤーの初期位置

    -const PhysicalStageDesc m_desc
    -const P2Body m_floor
}

BattleScene o-- IBattleManager
BattleManager o-- BattleTimer
BattleManager o-- BattlePlayerManager
BattleManager o-- PhysicalWorldManager
BattleManager o-- PhysicalMoveManager
BattlePlayerManager o-- BattlePlayer
PhysicalWorldManager o-- PhysicalCharacter
PhysicalWorldManager o-- PhysicalStage
PhysicalMoveManager o-- PhysicalMove
PhysicalCharacter <--> BattlePlayer

package ui {
interface IHolderUI
BattleUIManager --> IBattleManager
BattleUIManager o-- IHolderUI
IHolderUI <|.. HolderUI
BattleUIManager o-- PhysicalWorldUIManager
BattleUIManager o-- MoveUIManager
}
BattleScene o-- BattleUIManager

}

package chara {

interface IParameterizedCharacter {
    +const Kanji& kanji() const
    +bool isBurnedOut() const
    +const CharaParameters& params() const
    +const CharaParameters& initialParams() const
    +float hpRate() const

    +void damage(int amount)
}
class ParameterizedCharacter {
    -Kanji m_kanji
    -bool m_is_burned_out
    -const CharaParameters m_initial_params
    -CharaParameters m_params
}
IParameterizedCharacter <|.. ParameterizedCharacter

}

BattlePlayer o-- IParameterizedCharacter

@enduml
```
