
# 雑多メモ

## Sceneの追加手順

1. XXXXScene.hpp / cpp を追加、class XXXXScene : public KanjiScene
1. Scenes.hpp にincludeを追加
1. SceneStates.hpp の enum State に XXXX を追加
1. SequenseManager::initialize() に遷移方法を設定
