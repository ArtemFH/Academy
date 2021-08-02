# Lumen PHP Framework

Laravel Lumen is a stunningly fast PHP micro-framework for building web applications with expressive, elegant syntax. We
believe development must be an enjoyable, creative experience to be truly fulfilling. Lumen attempts to take the pain
out of development by easing common tasks used in the majority of web projects, such as routing, database abstraction,
queueing, and caching.

## Документация по группе роутинга /user

#### Все требуют авторизацию через токен, кроме "/register'

### Роутер POST "/register"

##### Request

    {
        "name": "test",
        "email": "test@test.ru",
        "password": "test"
    }

##### Responce

    {
        "data": null,
        "message": "Registered!"
    }

##### Responce с существующим email

    {
        "email": [
            "The email has already been taken."
        ]
    }

##### Responce если пустые поля name, email и password

    {
        "name": [
            "The name field is required."
        ],
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }

### Роутер POST "/login"

##### Request

    {
        "email": "test@test.ru",
        "password": "test"
    }

##### Responce если сущность существует в системе

    {
        "data": {
            "access_token": "Secret Access Token",
            "refresh_token": "Secret Refresh Token"
        },
        "message": "Logged!"
    }

##### Responce если сущность не существует в системе

    {
        "error": "Unauthorized"
    }

##### Responce если пустые поля email, password

    {
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }

### Роутер POST "/refreshAccessToken"

##### Request

    Передаём только refresh_token

##### Responce если токен рабочий

    {
         "data": {
             "access_token": "Secret Access Token"
         },
         "message": "Refreshed!"
    }

##### Responce если токен не рабочий

    {
        "status": "error",
        "message": "The token is invalid"
    }

### Роутер GET "/get-lists"

##### Request

    {
        "per_page": int,
        "page": int
    }

##### Responce если авторизирован

    {
        "data": {
            "attributes": [
                {
                    "id": int,
                    "title": string,
                    "todos": [
                        {
                            "id": int,
                            "title": string,
                            "completed": bool,
                            "checked": bool,
                            "date": date,
                        },
                        {
                            ...
                        }
                    ]
                },
                {
                    ...
                }
            ]
        },
        "message": "Received!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "data": {
            "attributes": []
        },
        "message": "Received!"
    }

## Документация по группе роутинга /list

### Роутер POST "/", "/create"

##### Request

    {
        "attributes": {
            "name": string
        }
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "count_tasks": int,
                "is_completed": bool,
                "is_closed": bool,
                "created_at": date,
                "updated_at": date
            }
        },
        "message": "Created!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

### Роутер GET "/", "/get-items"

##### Request

    {
        "filter":[
            ["id", ">", int],
            ["name", "=", string]
        ],
        "order": [
            [
                "id",
                "asc"
            ]
        ],
        "withs": [
            "task",
            "user"
        ],
        "per_page": int,
        "page": int
    }

##### Responce если авторизован

    {
        "data": {
            "items": [
                {
                    "id": int,
                    "name": string,
                    "count_tasks": int,
                    "is_completed": bool,
                    "is_closed": bool,
                    "created_at": date,
                    "updated_at": date,
                    "task": [
                        {
                            "id": int,
                            "name": string,
                            "list_id": int,
                            "executor_user_id": int,
                            "is_completed": bool,
                            "description": string,
                            "urgency": int,
                            "created_at": date,
                            "updated_at": date
                        }
                    ],
                    "user": [
                        {
                            "id": int,
                            "name": string,
                            "email": string,
                            "refresh_token": string,
                            "created_at": date,
                            "updated_at": date,
                            "pivot": {
                                "list_id": int,
                                "user_id": int
                            }
                        }
                    ]
                },
                {
                    ...
                }
            ]
        },
        "message": "Received!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер GET "/{id}", "/get-item/{id}"

##### Request

    {
        "withs": [
            "task"
            "user"
        ]
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "count_tasks": int,
                "is_completed": bool,
                "is_closed": bool,
                "created_at": date,
                "updated_at": date,
                "task": [
                    {
                        "id": int,
                        "name": string,
                        "list_id": int,
                        "executor_user_id": int,
                        "is_completed": bool,
                        "description": string,
                        "urgency": int,
                        "created_at": date,
                        "updated_at": date
                    }
                ],
                "user": [
                    {
                        "id": int,
                        "name": string,
                        "email": string,
                        "refresh_token": string,
                        "created_at": date,
                        "updated_at": date,
                        "pivot": {
                        "list_id": int,
                        "user_id": int
                        }
                    }
                ]
            }
        },
        "message": "Received!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер PUT "/{id}", "/update/{id}"

##### Request

    {
        "attributes":{
            "name": string
        }
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "count_tasks": int,
                "is_completed": bool,
                "is_closed": bool,
                "created_at": date,
                "updated_at": date
            }
        },
        "message": "Updated!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер DELETE "/{id}", "/delete/{id}"

##### Request

    {

    }

##### Responce если авторизован

    {
        "data": null,
        "message": "Deleted!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

## Документация по группе роутинга /task

### Роутер POST "/", "/create"

##### Request

    {
        "attributes": {
            "name": string,
            "list_id": int,
            "urgency": int,
            "description": string
        }
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "list_id": int,
                "executor_user_id": int,
                "is_completed": bool,
                "description": string,
                "urgency": int,
                "created_at": date,
                "updated_at": date
            }
        },
        "message": "Created!"
    }

##### Responce если не существует требуемая сущность

    {
        "attributes.list_id": [
            "The selected attributes.list id is invalid."
        ]
    }

##### Responce если urgency меньше 1 или больше 5

    {
        "attributes.urgency": [
            "The attributes.urgency may not be greater than 5."
        ]
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

### Роутер GET "/", "/get-items"

##### Request

    {
        "filter":[
            ["id", ">", int],
            ["name", "=", string]
        ],
        "order": [
            [
                "id",
                "asc"
            ]
        ],
        "withs": [
            "list",
            "user"
        ],
        "per_page": int,
        "page": int
    }

##### Responce если авторизован

    {
        "data": {
            "items": [
                {
                    "id": int,
                    "name": string,
                    "list_id": int,
                    "executor_user_id": int,
                    "is_completed": bool,
                    "description": string,
                    "urgency": int,
                    "created_at": date,
                    "updated_at": date,
                    "list": [
                        {
                            "id": int,
                            "name": string,
                            "count_tasks": int,
                            "is_completed": bool,
                            "is_closed": bool,
                            "created_at": date,
                            "updated_at": date
                        }
                    ],
                    "user": [
                        {
                            "id": int,
                            "name": string,
                            "email": string,
                            "refresh_token": string,
                            "created_at": date,
                            "updated_at": date,
                        }
                    ]
                },
                {
                    ...
                }
            ]
        },
        "message": "Received!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер GET "/{id}", "/get-item/{id}"

##### Request

    {
        "withs": [
            "list"
            "user"
        ]
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "list_id": int,
                "executor_user_id": int,
                "is_completed": bool,
                "description": string,
                "urgency": int,
                "created_at": date,
                "updated_at": date,
                "list": {
                    "id": int,
                    "name": string,
                    "count_tasks": int,
                    "is_completed": bool,
                    "is_closed": bool,
                    "created_at": date,
                    "updated_at": date
                },
                "user": {
                    "id": int,
                    "name": string,
                    "email": string,
                    "refresh_token": string,
                    "created_at": date,
                    "updated_at": date
                }
            }
        },
        "message": "Received!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер PUT "/{id}", "/update/{id}"

##### Request

    {
        "attributes":{
            "name": string,
            "is_completed": bool
        }
    }

##### Responce если авторизован

    {
        "data": {
            "attributes": {
                "id": int,
                "name": string,
                "list_id": int,
                "executor_user_id": int,
                "is_completed": bool,
                "description": string,
                "urgency": int,
                "created_at": date,
                "updated_at": date
            }
        },
        "message": "Updated!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }

### Роутер DELETE "/{id}", "/delete/{id}"

##### Request

    {

    }

##### Responce если авторизован

    {
        "data": null,
        "message": "Deleted!"
    }

##### Responce если не авторизован

    {
        "error": "Unauthorized."
    }

##### Responce если сущность не существует в системе

    {
        "status": "error",
        "message": "Not found"
    }
