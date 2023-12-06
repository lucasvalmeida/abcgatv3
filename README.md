# Infinity Run!

Atividade 3: 
Jogo de corrida infinita.

## Autores

- Lucas Venâncio de Almeida    RA: 21058312
- Lucas Guerra Silvestre       RA: 11201721549

## Descrição do protótipo

O protótipo é de um jogo de corrida infinita.

## Principais implementações

Utilizamos como base o projeto Starfield apresentado em aula.

Alterações feitas no Window::onCreate():
```cpp
void Window::onCreate() {
// restante do código

    // Novo objeto importado
    m_carro.loadObj(assetsPath + "modelo_carro.obj");
    m_carro.setupVAO(m_program);

    // Definindo posições iniciais dos objetos
    carro0.m_position = glm::vec3(0.0f, -0.02f, -0.001f);
    carro1.m_position = glm::vec3(glm::linearRand(-0.05f, 0.05f), -0.02f, -0.005f);

// restante do código
}
```

Alterações feitas no Window::randomizeStar():
```cpp
void Window::randomizeStar(Star &star) {
// restante do código

    // Sorteia um número (0,1)
    std::uniform_int_distribution<int> distPosSide(0, 1);
    int sorteia_lado = distPosSide(m_randomEngine);
    std::uniform_real_distribution<float> distPosX;

    // Define o lado da pista que o cubo irá aparecer
    if (sorteia_lado == 0) {
        distPosX = std::uniform_real_distribution<float>(25.0f, 40.0f);
      } else {
        distPosX = std::uniform_real_distribution<float>(-40.0f, -25.0f);
      } 

    // Passa todas posições do cubo
      star.m_position =
            glm::vec3(distPosX(m_randomEngine), distPosY(m_randomEngine),
                      distPosZ(m_randomEngine));

// restante do código
}
```

Alterações feitas no Window::onUpdate():
```cpp
void Window::onUpdate() {
// restante do código

    // Define a direção e velocidade do carro no eixo Z
    carro1.m_position.z += deltaTime * 0.001f * 0.5f;
        if (carro1.m_position.z > 0.01f) {
          randomizeStar(star);
          carro1.m_position.z = -4.0f; // Back to -100
        }

// restante do código
}
```

Alterações feitas no Window::onPaint():
```cpp
void Window::onPaint() {
// restante do código

    // Desenha pista:
    pista1.m_position = glm::vec3(0.0f,-10.0f,-30.0f);
    pista1.m_rotationAxis = glm::vec3(0.2f,0.8f,0.3f);
    
    glm::mat4 matrizPista{1.0f};
    matrizPista = glm::translate(matrizPista, pista1.m_position);
    matrizPista = glm::scale(matrizPista, glm::vec3(60.0f,2.0f,240.0f));

    pista1.m_rotationAxis = glm::vec3(1.0f,0.0f,0.0f);
    angulo_pista = glm::radians(20.0f);
    matrizPista = glm::rotate(matrizPista, angulo_pista, pista1.m_rotationAxis);

    abcg::glUniformMatrix4fv(modelMatrixLoc, 1, GL_FALSE, &matrizPista[0][0]);
    m_model.render();

    // Desenha carro0:
    glm::mat4 matrizCarro0{1.0f};
    matrizCarro0 = glm::translate(matrizCarro0, carro0.m_position);
    matrizCarro0 = glm::scale(matrizCarro0, glm::vec3(0.02f,0.02f,0.02f));

    angulo_carro0 = glm::radians(0.0f);
    matrizCarro0 = glm::rotate(matrizCarro0, angulo_carro0, pista0.m_rotationAxis);

    abcg::glUniformMatrix4fv(modelMatrixLoc, 1, GL_FALSE, &matrizCarro0[0][0]);
    m_carro.render();

    // Desenha carro1:
    glm::mat4 matrizCarro1{1.0f};
    matrizCarro1 = glm::translate(matrizCarro1, carro1.m_position);
    matrizCarro1 = glm::scale(matrizCarro1, glm::vec3(0.02f,0.02f,0.02f));

    angulo_carro1 = glm::radians(20.0f);
    matrizCarro1 = glm::rotate(matrizCarro1, angulo_carro1, pista1.m_rotationAxis);

    abcg::glUniformMatrix4fv(modelMatrixLoc, 1, GL_FALSE, &matrizCarro1[0][0]);
    m_carro.render();

// restante do código
}
```

Implementações feitas no Window::onEvent():
```cpp
void Window::onEvent(SDL_Event const &event) {
// restante do código

    // Captura entradas do teclado para mover o carro 0 para direita e esquerda
    if (event.type == SDL_KEYDOWN) {
        if (event.key.keysym.sym == SDLK_LEFT || event.key.keysym.sym == SDLK_a) {
            carro0.m_position.x -= 0.01f;
        }
        if (event.key.keysym.sym == SDLK_RIGHT || event.key.keysym.sym == SDLK_d) {
            carro0.m_position.x += 0.01f;
        }
    }

// restante do código
}
```
