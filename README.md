 #include <raylib.h>
#include <iostream>
#include <string>

using namespace std;

int main()
{
    const int screenWidth = 800;
    const int screenHeight = 450;

    InitWindow(screenWidth, screenHeight, "Pelota rebotando - Raylib");
    SetTargetFPS(60);

    Vector2 ballPosition = { screenWidth / 2.0f, screenHeight / 2.0f };
    float ballRadius = 20.0f;
    Vector2 ballSpeed = { 300.0f, 250.0f }; // píxeles por segundo
    Color ballColor = MAROON;

    while (!WindowShouldClose())
    {
        float dt = GetFrameTime();

        // Actualizar posición
        ballPosition.x += ballSpeed.x * dt;
        ballPosition.y += ballSpeed.y * dt;

        // Colisiones con los bordes: invertir velocidad y corregir posición
        if (ballPosition.x - ballRadius <= 0.0f)
        {
            ballPosition.x = ballRadius;
            ballSpeed.x *= -1.0f;
        }
        else if (ballPosition.x + ballRadius >= screenWidth)
        {
            ballPosition.x = screenWidth - ballRadius;
            ballSpeed.x *= -1.0f;
        }

        if (ballPosition.y - ballRadius <= 0.0f)
        {
            ballPosition.y = ballRadius;
            ballSpeed.y *= -1.0f;
        }
        else if (ballPosition.y + ballRadius >= screenHeight)
        {
            ballPosition.y = screenHeight - ballRadius;
            ballSpeed.y *= -1.0f;
        }

        // Reiniciar con R
        if (IsKeyPressed(KEY_R))
        {
            ballPosition = { screenWidth / 2.0f, screenHeight / 2.0f };
            ballSpeed = { 300.0f, 250.0f };
        }

        BeginDrawing();
        ClearBackground(RAYWHITE);
        DrawCircleV(ballPosition, ballRadius, ballColor);

        // Texto de ayuda y "Hola Mundo"
        DrawText("Pulsa R para reiniciar", 10, 10, 20, DARKGRAY);
        const char *hola = "Hola Mundo";
        int fontSize = 24;
        int textWidth = MeasureText(hola, fontSize);
        DrawText(hola, (screenWidth - textWidth) / 2, 40, fontSize, BLACK);

        DrawFPS(10, screenHeight - 30);
        EndDrawing();
    }

    CloseWindow();
    return 0;
}
