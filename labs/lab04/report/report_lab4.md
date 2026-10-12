`---`  
`author:`  
  `name: Бессонов Тимофей Романович`  
  `email: mamkar228@yandex.ru`  
  `affiliation:`  
    `- name: Российский университет дружбы народов`  
      `country: Российская Федерация`  
      `city: Москва`  
      `group: НКАбд-01-25`  
      `student-id: 1032253551`

`title: "Отчёт по лабораторной работе № 4"`  
`subtitle: "Дисциплина: Архитектура Компьютера и Операционные системы"`  
`license: "CC BY"`  
`abstract: |`  
  `В данном отчёте представлены результаты выполнения лабораторной работы № 4 «Продвинутое использование git».`  
  `Рассмотрены принципы организации ветвления по методологии Gitflow, концепция семантического`  
  `версионирования (SemVer) и правила форматирования Conventional Commits.`  
  `Настроено окружение Node.js (pnpm, commitizen, standard-changelog), инициализирован git-flow`  
  `в тестовом репозитории, создан жизненный цикл релиза с публикацией тегов и журнала изменений на GitHub.`  
`keywords: [лабораторная работа, РУДН, Git, Gitflow, SemVer, Conventional Commits, commitizen, GitHub]`  
`---`

`# Введение`

`## Актуальность`

`При разработке масштабных проектов и поддержке версионируемого программного обеспечения критически важно соблюдать строгую дисциплину ветвления и оформления истории изменений. Модель ветвления Gitflow Workflow позволяет изолировать разработку нового функционала, подготовку релизов и срочные исправления (hotfix), а соблюдение стандарта Conventional Commits и семантического версионирования обеспечивает автоматизацию генерации журналов изменений (Changelog) и корректное управление версиями.`

`## Цель работы`

`Получение практических навыков правильной организации рабочего процесса в репозиториях Git с использованием модели Gitflow, семантического версионирования и общепринятых коммитов (Conventional Commits).`

`## Задачи`

``1. Установить расширение `git-flow`, среду выполнения `Node.js` и менеджер пакетов `pnpm`.``  
``2. Установить и настроить утилиты автоматизации коммитов и генерации логов (`commitizen`, `cz-conventional-changelog`, `standard-changelog`).``  
``3. Создать тестовый репозиторий `git-extended` и опубликовать его на GitHub.``  
``4. Инициализировать в репозитории структуру `git flow` (основные ветки `master` и `develop`, префикс версий `v`).``  
``5. Настроить конфигурационный файл `package.json` под стандарт Conventional Commits.``  
``6. Отработать жизненный цикл выпуска первичного релиза `v1.0.0` с генерацией `CHANGELOG.md` и публикацией на GitHub через CLI `gh`.``  
``7. Смоделировать работу с функциональной веткой (`feature`) и выпуск последующего релиза `v1.2.3`.``

`## Объект и предмет исследования`

`- **Объект исследования:** система контроля версий Git и платформа GitHub.`  
``- **Предмет исследования:** утилита `git-flow`, стандарты SemVer и Conventional Commits, пакеты экосистемы Node.js (`commitizen`, `standard-changelog`).``

`## Схема выполнения работы`

`На рис. 1 приведена общая схема последовательности шагов лабораторной работы.`

```` ```mermaid ````  
`graph TD`  
    `A([Начало]) --> B[Установка gitflow, Node.js, pnpm]`  
    `B --> C[Установка commitizen и standard-changelog]`  
    `C --> D[Создание репозитория git-extended]`  
    `D --> E[Инициализация git-flow init]`  
    `E --> F[Релиз v1.0.0 и создание CHANGELOG.md]`  
    `F --> G[Разработка в ветке feature]`  
    `G --> H[Релиз v1.2.3 и публикация через gh]`  
    `H --> J([Конец])`

*Рис. 1\. Блок-схема выполнения лабораторной работы*

# **Теоретические сведения**

## **Рабочий процесс Gitflow**

Модель Gitflow предполагает использование выделенных веток под конкретные фазы жизненного цикла проекта:

> * master — хранит стабильную историю релизов, каждый коммит помечается тегом версии.  
> * develop — основная интеграционная ветка, объединяющая завершённые функции.  
> * feature/\* — ветки разработки новых функций, создаются от develop и вливаются обратно в develop.  
> * release/\* — ветки подготовки выпуска новой версии (стабилизация кода, подготовка документации, обновление changelog), создаются от develop и вливаются в master и develop.  
> * hotfix/\* — экстренные исправления продакшн-ошибок, ответвляются напрямую от master и затем вливаются в master и develop.

## **Семантическое версионирование (SemVer)**

Номер версии задаётся форматом МАЖОРНАЯ.МИНОРНАЯ.ПАТЧ (X.Y.Z):

> * **МАЖОРНАЯ (MAJOR):** увеличивается при наличии обратно несовместимых изменений API (BREAKING CHANGE).  
> * **МИНОРНАЯ (MINOR):** увеличивается при добавлении обратной совместимой новой функциональности (feat:).  
> * **ПАТЧ (PATCH):** увеличивается при исправлении ошибок без изменения API (fix:).

## **Спецификация Conventional Commits**

Формат сообщения коммита:  
`<тип>(<область>): <краткое описание>`

`[необязательное тело с мотивацией изменений]`

`[необязательный подвал с ссылками на задачи и BREAKING CHANGE]`

Основные типы коммитов: feat (новый функционал), fix (исправление ошибок), docs (документация), style (форматирование), refactor (рефакторинг), perf (производительность), test (тесты), chore (рутинные задачи/сборка).

# **Выполнение лабораторной работы**

## **1\. Установка программного обеспечения**

Подключили сторонний репозиторий Copr и установили пакет gitflow, а также платформу Node.js и менеджер pnpm:  
`sudo dnf copr enable -y elegos/gitflow`  
`sudo dnf install -y gitflow nodejs pnpm`

Выполнили первоначальную настройку окружения pnpm и применили изменения переменных среды:  
`pnpm setup`  
`source ~/.bashrc`

Глобально установили вспомогательные утилиты для работы со стандартами Conventional Commits:  
`pnpm add -g commitizen cz-conventional-changelog standard-changelog`

\!\[\[01-install-software.png\]\] *Рис. 2\. Установка утилит gitflow, commitizen и standard-changelog*

## **2\. Создание и базовая настройка репозитория**

Создали локальный каталог git-extended, выполнили первый коммит и связали с удалённым репозиторием на GitHub:  
`mkdir ~/git-extended && cd ~/git-extended`  
`git init`  
`git commit --allow-empty -m "chore: initial commit"`  
`git remote add origin git@github.com:trbessonov/git-extended.git`  
`git branch -M master`  
`git push -u origin master`

Сконфигурировали структуру проекта Node.js с поддержкой адаптера commitizen:  
`pnpm init`

В файле package.json указали авторские метаданные, лицензию CC-BY-4.0 и путь к адаптеру cz-conventional-changelog:  
`{`  
  `"name": "git-extended",`  
  `"version": "1.0.0",`  
  `"description": "Git repository for educational purposes",`  
  `"main": "index.js",`  
  `"repository": "git@github.com:trbessonov/git-extended.git",`  
  `"author": "Тимофей Бессонов <mamkar228@yandex.ru>",`  
  `"license": "CC-BY-4.0",`  
  `"config": {`  
    `"commitizen": {`  
      `"path": "cz-conventional-changelog"`  
    `}`  
  `}`  
`}`

Зафиксировали созданный конфигурационный файл с помощью утилиты интерактивного коммита git-cz:  
`git add package.json`  
`git cz`  
`git push origin master`

\!\[\[02-package-json-cz.png\]\] *Рис. 3\. Форматирование коммита через интерфейс commitizen (git-cz)*

## **3\. Конфигурация git-flow и выпуск релиза 1.0.0**

Инициализировали в проекте структуру Gitflow, указав ветку по умолчанию для релизов master, ветку разработки develop и префикс для тегов версий v:  
`git flow init -d`  
`git config gitflow.prefix.versiontag v`  
`git branch --set-upstream-to=origin/develop develop 2>/dev/null || true`  
`git push --all`

Запустили процедуру создания первого релиза 1.0.0:  
`git flow release start 1.0.0`

Сгенерировали журнал изменений CHANGELOG.md на основе истории коммитов:  
`standard-changelog --first-release`  
`git add CHANGELOG.md`  
`git commit -am 'chore(site): add changelog'`

Завершили релиз, выполнив автоматическое слияние в ветки master и develop, и опубликовали все изменения с тегами на GitHub:  
`git flow release finish 1.0.0`  
`git push --all`  
`git push --tags`

Создали официальный релиз на GitHub через утилиту gh:  
`gh release create v1.0.0 -F CHANGELOG.md`

\!\[\[03-release-v100.png\]\] *Рис. 4\. Успешный выпуск релиза v1.0.0 на GitHub*

## **4\. Разработка функционала и выпуск релиза 1.2.3**

Отработали сценарий создания функциональной ветки для внедрения новой возможности:  
`git flow feature start sample_feature`  
`echo "console.log('Advanced Git Feature');" > app.js`  
`git add app.js`  
`git cz`  
`git flow feature finish sample_feature`

Запустили выпуск минорного релиза 1.2.3:  
`git flow release start 1.2.3`

Обновили версию в package.json на "version": "1.2.3", перегенерировали журнал изменений и завершили ветку релиза:  
`standard-changelog`  
`git add CHANGELOG.md package.json`  
`git commit -am 'chore(release): update changelog for 1.2.3'`  
`git flow release finish 1.2.3`

Отправили данные в центральный репозиторий и зафиксировали релиз:  
`git push --all`  
`git push --tags`  
`gh release create v1.2.3 -F CHANGELOG.md`

\!\[\[04-release-v123.png\]\] *Рис. 5\. Выпуск релиза v1.2.3 с автоматически обновлённым журналом изменений*

# **Заключение**

> 1. Освоена методология ветвления Gitflow Workflow, закреплены навыки работы с ветками функционала (feature), релизов (release) и стабильного кода (master).  
> 2. Изучены принципы семантического версионирования (SemVer), регламентирующие правила инкремента мажорных, минорных и патч-номеров версий.  
> 3. Настроена инфраструктура автоматизации коммитов по стандарту Conventional Commits с использованием commitizen и автоматическая генерация документа CHANGELOG.md с помощью standard-changelog.  
> 4. Отработан полный цикл подготовки и публикации версионированных релизов проекта на GitHub с использованием интерфейса командной строки gh.

# **Список литературы**

> 1. Driessen, V. A successful Git branching model. URL: https\://nvie.com/posts/a-successful-git-branching-model/  
> 2. Semantic Versioning 2.0.0 Specification. URL: https\://semver.org/  
> 3. Conventional Commits 1.0.0 Specification. URL: https\://www\.conventionalcommits.org/  
> 4. Chacon, S. Pro Git / S. Chacon, B. Straub. – 2nd ed. – Apress, 2014\. – 456 p.

