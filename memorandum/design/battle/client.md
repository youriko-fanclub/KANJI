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
    BattleManager m_battle_manager;
    BattleUIManager m_battle_ui_manager;
}

interface IBattleManager
class BattleManager {
    PanelGrid m_panel_grid;
    PlayerCursorManager m_player_cursor_manager;
    CharacterHolderManager m_character_holder_manager;
}
BattleManager ..|> IBattleManager
class BattleTimer

class BattlePlayerManager
class BattlePlayer

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

interface IParameterizedCharacter {}
IParameterizedCharacter <|.. ParameterizedCharacter

}

BattlePlayer o-- IParameterizedCharacter

@enduml
```
