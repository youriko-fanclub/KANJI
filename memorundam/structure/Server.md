~~~plantuml
@startuml DB.png
title <size:18> DB </size>

class User {
    // Core
    int user_id
    string icon
    string name
    // Record
    int wonNum
    int rate
    // Social
    vector<PlayerProfileID> friendList
    string friendCode
    // Additional
    string message
}

@enduml
~~~