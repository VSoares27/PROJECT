<script lang="ts">
    import { goto } from '$app/navigation'    
    
    class Coordenada {
        linha : number
        coluna : number
    }
    
    
    class EstadoJogo {
        posicaoPersonagem : Coordenada
        posicaoObjetivo : Coordenada
        mapa : number[][]
    }
    
    
    function inicializarJogo() : EstadoJogo {
        let personagemA: Coordenada = new Coordenada()
        personagemA.linha = 5
        personagemA.coluna = 0

        let objetivo : Coordenada = new Coordenada()
        objetivo.linha = 5
        objetivo.coluna = 9
        
        let mapa : number[][] = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
                                 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ]
    
    let estado : EstadoJogo = new EstadoJogo()
    estado.posicaoPersonagem = personagemA
    estado.posicaoObjetivo = objetivo
    estado.mapa = mapa
    
    return estado;
    
}


function houveColisao(posicao : Coordenada, jogo : EstadoJogo) : boolean {
    return (posicao.linha < 0 || posicao.coluna < 0)
    || (posicao.linha >= jogo.mapa.length || posicao.coluna >= jogo.mapa[0].length)
    || jogo.mapa[posicao.linha][posicao.coluna] == 1
}

function onKeyDown(evento) : void {
    let novaPosicao = new Coordenada()
    novaPosicao.linha = jogo.posicaoPersonagem.linha
    novaPosicao.coluna = jogo.posicaoPersonagem.coluna

    switch(evento.keyCode) {
        case 38: // up / cima
        novaPosicao.linha--
        break;
        case 37: // left / esquerda
        novaPosicao.coluna--
        break;
        case 39: // right / direita
        novaPosicao.coluna++
        break;
        case 40: // down / baixo
        novaPosicao.linha++
        break;
    }
    
    if (novaPosicao.linha == jogo.posicaoObjetivo.linha && novaPosicao.coluna == jogo.posicaoObjetivo.coluna) {
        alert("Parabéns, você chegou ao objetivo")
        goto("/")
    }
    
    if(!houveColisao(novaPosicao, jogo)) {
        jogo.posicaoPersonagem = novaPosicao
    }
}

    let jogo : EstadoJogo = inicializarJogo()


</script>

<h1>MOVIMENTE O PERSONAGEM ATÉ O OBJETIVO FINAL (QUADRADO AMARELO)</h1>

<table>
    {#each jogo.mapa as linha, i}
        <tr>
            {#each linha as celula, j}
                {#if i == jogo.posicaoPersonagem.linha &&  j == jogo.posicaoPersonagem.coluna}
                    <td class="celula personagem"></td>
                {:else if i == jogo.posicaoObjetivo.linha &&  j == jogo.posicaoObjetivo.coluna}
                    <td class="celula objetivo"></td>
                {:else if jogo.mapa[i][j] == 0}
                    <td class="celula"></td>
                {:else}
                    <td class="celula bloco"></td>
                {/if}
            {/each}
        </tr>
    {/each}    
</table>

<br />

<a class="menu" href="/">Voltar ao Menu</a>


<svelte:window on:keydown|preventDefault={onKeyDown} />

