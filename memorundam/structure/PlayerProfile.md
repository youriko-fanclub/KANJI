~~~plantuml
@startuml
title <size:18> Structure of PlayerProfile </size>

package domain {
    class DomainPlayerProfile
    class DomainPlayerProfileCore
}
package interface {
    interface IPlayerProfileCore {
        +int id();
        +png icon();
        +std::string name();
    }
    interface IPlayerProfile {
        +ICore core();
        +std::string message();
        +std::vector<std::string> friendList();
        +std::string frinedCode();
        +int wonNum();
        +int rate();
    }
}
package userdata {
    class PlayerProfileCore {
        -int m_id;
        -png m_icon;
        -std::string m_name;
    }
    class PlayerProfile {
        -PlayerProfileCore m_core;
        -std::string m_message;
        -std::vector<std::string> m_friendList;
        -std::string m_friendCode;
        -int m_wonNum;
        -int m_rate;
    }
}
IPlayerProfile --|> IPlayerProfileCore
PlayerProfile o-- PlayerProfileCore
PlayerProfile .right.|> IPlayerProfile
PlayerProfileCore .right.|> IPlayerProfileCore
DomainPlayerProfile ..|> IPlayerProfile
DomainPlayerProfile --> PlayerProfile
DomainPlayerProfileCore --> PlayerProfileCore
DomainPlayerProfile o-- DomainPlayerProfileCore

class PlayerInGame {
    // ID、アイコン、名前
    -PlayerProfileCore m_core
    // 手番順
    -int m_moveOrder
    // 実質所属陣営 - 勝敗を連帯する色
    -Color m_homeColor
    // 名目所属陣営 - 置く石の色
    -Color m_nameColor
    // デッキ番号
    -int m_deckId
}
PlayerInfoInGame o-up- PlayerProfileCore

@enduml
~~~