import React, { useState, useEffect, useRef, useCallback } from 'react';
import * as LucideIcons from 'lucide-react'; // Alterado para importar todos os ícones

// Desestruturando os ícones necessários do objeto LucideIcons
const { Volume2, ChevronLeft, ChevronRight, Paintbrush, Puzzle, BookOpen, Divide } = LucideIcons;

// Data for words and alphabet (Portuguese and English)
const wordsData = {
  pt: {
    alphabet: [
      { letter: 'A', sound: 'A', word: 'Abacaxi', image: 'https://placehold.co/150x150/FFD700/000000?text=Abacaxi', syllables: ['A', 'ba', 'ca', 'xi'] },
      { letter: 'B', sound: 'Bê', word: 'Bola', image: 'https://placehold.co/150x150/87CEEB/000000?text=Bola', syllables: ['Bo', 'la'] },
      { letter: 'C', sound: 'Cê', word: 'Casa', image: 'https://placehold.co/150x150/FF6347/000000?text=Casa', syllables: ['Ca', 'sa'] },
      { letter: 'D', sound: 'Dê', word: 'Dado', image: 'https://placehold.co/150x150/98FB98/000000?text=Dado', syllables: ['Da', 'do'] },
      { letter: 'E', sound: 'E', word: 'Elefante', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Elefante', syllables: ['E', 'le', 'fan', 'te'] },
      { letter: 'F', sound: 'Éfe', word: 'Foca', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Foca', syllables: ['Fo', 'ca'] },
      { letter: 'G', sound: 'Gê', word: 'Gato', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Gato', syllables: ['Ga', 'to'] },
      { letter: 'H', sound: 'Agá', word: 'Hipopótamo', image: 'https://placehold.co/150x150/F0E68C/000000?text=Hipopótamo', syllables: ['Hi', 'po', 'pó', 'ta', 'mo'] },
      { letter: 'I', sound: 'I', word: 'Igreja', image: 'https://placehold.co/150x150/AFEEEE/000000?text=Igreja', syllables: ['I', 'gre', 'ja'] },
      { letter: 'J', sound: 'Jota', word: 'Jacaré', image: 'https://placehold.co/150x150/FFB6C1/000000?text=Jacaré', syllables: ['Ja', 'ca', 'ré'] },
      { letter: 'K', sound: 'Cá', word: 'Kiwi', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Kiwi', syllables: ['Ki', 'wi'] },
      { letter: 'L', sound: 'Éle', word: 'Leão', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Leão', syllables: ['Le', 'ão'] },
      { letter: 'M', sound: 'Ême', word: 'Macaco', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Macaco', syllables: ['Ma', 'ca', 'co'] },
      { letter: 'N', sound: 'Êne', word: 'Nuvem', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Nuvem', syllables: ['Nu', 'vem'] },
      { letter: 'O', sound: 'O', word: 'Ovelha', image: 'https://placehold.co/150x150/FFFACD/000000?text=Ovelha', syllables: ['O', 've', 'lha'] },
      { letter: 'P', sound: 'Pê', word: 'Pato', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Pato', syllables: ['Pa', 'to'] },
      { letter: 'Q', sound: 'Quê', word: 'Queijo', image: 'https://placehold.co/150x150/F0E68C/000000?text=Queijo', syllables: ['Quei', 'jo'] },
      { letter: 'R', sound: 'Êrre', word: 'Rato', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Rato', syllables: ['Ra', 'to'] },
      { letter: 'S', sound: 'Ésse', word: 'Sol', image: 'https://placehold.co/150x150/FFD700/000000?text=Sol', syllables: ['Sol'] },
      { letter: 'T', sound: 'Tê', word: 'Trem', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Trem', syllables: ['Trem'] },
      { letter: 'U', sound: 'U', word: 'Uva', image: 'https://placehold.co/150x150/DA70D6/000000?text=Uva', syllables: ['U', 'va'] },
      { letter: 'V', sound: 'Vê', word: 'Vaca', image: 'https://placehold.co/150x150/98FB98/000000?text=Vaca', syllables: ['Va', 'ca'] },
      { letter: 'W', sound: 'Dáblio', word: 'Webcam', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Webcam', syllables: ['Web', 'cam'] },
      { letter: 'X', sound: 'Xis', word: 'Xícara', image: 'https://placehold.co/150x150/FF6347/000000?text=Xícara', syllables: ['Xí', 'ca', 'ra'] },
      { letter: 'Y', sound: 'Ípsilon', word: 'Yakult', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Yakult', syllables: ['Ya', 'kult'] },
      { letter: 'Z', sound: 'Zê', word: 'Zebra', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Zebra', syllables: ['Ze', 'bra'] },
    ],
    words: [
      { word: 'Cachorro', image: 'https://placehold.co/150x150/DEB887/000000?text=Cachorro', syllables: ['Ca', 'cho', 'rro'] },
      { word: 'Flor', image: 'https://placehold.co/150x150/FF69B4/000000?text=Flor', syllables: ['Flor'] },
      { word: 'Carro', image: 'https://placehold.co/150x150/FF4500/000000?text=Carro', syllables: ['Ca', 'rro'] },
      { word: 'Árvore', image: 'https://placehold.co/150x150/228B22/000000?text=Árvore', syllables: ['Ár', 'vo', 're'] },
      { word: 'Sapato', image: 'https://placehold.co/150x150/CD853F/000000?text=Sapato', syllables: ['Sa', 'pa', 'to'] },
      { word: 'Janela', image: 'https://placehold.co/150x150/87CEEB/000000?text=Janela', syllables: ['Ja', 'ne', 'la'] },
      { word: 'Livro', image: 'https://placehold.co/150x150/D2B48C/000000?text=Livro', syllables: ['Li', 'vro'] },
      { word: 'Cadeira', image: 'https://placehold.co/150x150/A0522D/000000?text=Cadeira', syllables: ['Ca', 'dei', 'ra'] },
      { word: 'Mesa', image: 'https://placehold.co/150x150/F4A460/000000?text=Mesa', syllables: ['Me', 'sa'] },
      { word: 'Pássaro', image: 'https://placehold.co/150x150/B0E0E6/000000?text=Pássaro', syllables: ['Pás', 'sa', 'ro'] },
      { word: 'Sol', image: 'https://placehold.co/150x150/FFD700/000000?text=Sol', syllables: ['Sol'] },
      { word: 'Lua', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Lua', syllables: ['Lu', 'a'] },
      { word: 'Estrela', image: 'https://placehold.co/150x150/FFFF00/000000?text=Estrela', syllables: ['Es', 'tre', 'la'] },
      { word: 'Nuvem', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Nuvem', syllables: ['Nu', 'vem'] },
      { word: 'Chuva', image: 'https://placehold.co/150x150/6495ED/000000?text=Chuva', syllables: ['Chu', 'va'] },
      { word: 'Vento', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Vento', syllables: ['Ven', 'to'] },
      { word: 'Fogo', image: 'https://placehold.co/150x150/FF4500/000000?text=Fogo', syllables: ['Fo', 'go'] },
      { word: 'Água', image: 'https://placehold.co/150x150/4682B4/000000?text=Água', syllables: ['Á', 'gua'] },
      { word: 'Terra', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Terra', syllables: ['Ter', 'ra'] },
      { word: 'Montanha', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Montanha', syllables: ['Mon', 'ta', 'nha'] },
      { word: 'Rio', image: 'https://placehold.co/150x150/00BFFF/000000?text=Rio', syllables: ['Ri', 'o'] },
      { word: 'Mar', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Mar', syllables: ['Mar'] },
      { word: 'Praia', image: 'https://placehold.co/150x150/F4A460/000000?text=Praia', syllables: ['Prai', 'a'] },
      { word: 'Areia', image: 'https://placehold.co/150x150/F0E68C/000000?text=Areia', syllables: ['A', 'rei', 'a'] },
      { word: 'Peixe', image: 'https://placehold.co/150x150/FF6347/000000?text=Peixe', syllables: ['Pei', 'xe'] },
      { word: 'Pássaro', image: 'https://placehold.co/150x150/B0E0E6/000000?text=Pássaro', syllables: ['Pás', 'sa', 'ro'] },
      { word: 'Borboleta', image: 'https://placehold.co/150x150/FFB6C1/000000?text=Borboleta', syllables: ['Bor', 'bo', 'le', 'ta'] },
      { word: 'Abelha', image: 'https://placehold.co/150x150/FFD700/000000?text=Abelha', syllables: ['A', 'be', 'lha'] },
      { word: 'Formiga', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Formiga', syllables: ['For', 'mi', 'ga'] },
      { word: 'Aranha', image: 'https://placehold.co/150x150/696969/FFFFFF?text=Aranha', syllables: ['A', 'ra', 'nha'] },
      { word: 'Cobra', image: 'https://placehold.co/150x150/32CD32/000000?text=Cobra', syllables: ['Co', 'bra'] },
      { word: 'Leão', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Leão', syllables: ['Le', 'ão'] },
      { word: 'Tigre', image: 'https://placehold.co/150x150/FF8C00/000000?text=Tigre', syllables: ['Ti', 'gre'] },
      { word: 'Elefante', image: 'https://placehold.co/150x150/D3D3D3/000000?text=Elefante', syllables: ['E', 'le', 'fan', 'te'] },
      { word: 'Girafa', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Girafa', syllables: ['Gi', 'ra', 'fa'] },
      { word: 'Zebra', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Zebra', syllables: ['Ze', 'bra'] },
      { word: 'Macaco', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Macaco', syllables: ['Ma', 'ca', 'co'] },
      { word: 'Urso', image: 'https://placehold.co/150x150/A0522D/000000?text=Urso', syllables: ['Ur', 'so'] },
      { word: 'Coelho', image: 'https://placehold.co/150x150/F0F8FF/000000?text=Coelho', syllables: ['Co', 'e', 'lho'] },
      { word: 'Raposa', image: 'https://placehold.co/150x150/CD5C5C/000000?text=Raposa', syllables: ['Ra', 'po', 'sa'] },
      { word: 'Lobo', image: 'https://placehold.co/150x150/708090/FFFFFF?text=Lobo', syllables: ['Lo', 'bo'] },
      { word: 'Veado', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Veado', syllables: ['Ve', 'a', 'do'] },
      { word: 'Coruja', image: 'https://placehold.co/150x150/D2B48C/000000?text=Coruja', syllables: ['Co', 'ru', 'ja'] },
      { word: 'Águia', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Águia', syllables: ['Á', 'guia'] },
      { word: 'Pato', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Pato', syllables: ['Pa', 'to'] },
      { word: 'Galinha', image: 'https://placehold.co/150x150/F0E68C/000000?text=Galinha', syllables: ['Ga', 'li', 'nha'] },
      { word: 'Galo', image: 'https://placehold.co/150x150/FF6347/000000?text=Galo', syllables: ['Ga', 'lo'] },
      { word: 'Ovelha', image: 'https://placehold.co/150x150/FFFACD/000000?text=Ovelha', syllables: ['O', 've', 'lha'] },
      { word: 'Porco', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Porco', syllables: ['Por', 'co'] },
      { word: 'Cavalo', image: 'https://placehold.co/150x150/CD853F/000000?text=Cavalo', syllables: ['Ca', 'va', 'lo'] },
      { word: 'Vaca', image: 'https://placehold.co/150x150/98FB98/000000?text=Vaca', syllables: ['Va', 'ca'] },
      { word: 'Cabra', image: 'https://placehold.co/150x150/D2B48C/000000?text=Cabra', syllables: ['Ca', 'bra'] },
      { word: 'Gato', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Gato', syllables: ['Ga', 'to'] },
      { word: 'Cão', image: 'https://placehold.co/150x150/DEB887/000000?text=Cão', syllables: ['Cão'] },
      { word: 'Boneca', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Boneca', syllables: ['Bo', 'ne', 'ca'] },
      { word: 'Carro', image: 'https://placehold.co/150x150/FF4500/000000?text=Carro', syllables: ['Ca', 'rro'] },
      { word: 'Bola', image: 'https://placehold.co/150x150/87CEEB/000000?text=Bola', syllables: ['Bo', 'la'] },
      { word: 'Bicicleta', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Bicicleta', syllables: ['Bi', 'ci', 'cle', 'ta'] },
      { word: 'Avião', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Avião', syllables: ['A', 'vi', 'ão'] },
      { word: 'Trem', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Trem', syllables: ['Trem'] },
      { word: 'Navio', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Navio', syllables: ['Na', 'vi', 'o'] },
      { word: 'Barco', image: 'https://placehold.co/150x150/4682B4/000000?text=Barco', syllables: ['Bar', 'co'] },
      { word: 'Foguete', image: 'https://placehold.co/150x150/8A2BE2/FFFFFF?text=Foguete', syllables: ['Fo', 'gue', 'te'] },
      { word: 'Casa', image: 'https://placehold.co/150x150/FF6347/000000?text=Casa', syllables: ['Ca', 'sa'] },
      { word: 'Prédio', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Prédio', syllables: ['Pré', 'dio'] },
      { word: 'Escola', image: 'https://placehold.co/150x150/98FB98/000000?text=Escola', syllables: ['Es', 'co', 'la'] },
      { word: 'Hospital', image: 'https://placehold.co/150x150/DC143C/FFFFFF?text=Hospital', syllables: ['Hos', 'pi', 'tal'] },
      { word: 'Parque', image: 'https://placehold.co/150x150/32CD32/000000?text=Parque', syllables: ['Par', 'que'] },
      { word: 'Loja', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Loja', syllables: ['Lo', 'ja'] },
      { word: 'Mercado', image: 'https://placehold.co/150x150/F0E68C/000000?text=Mercado', syllables: ['Mer', 'ca', 'do'] },
      { word: 'Restaurante', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Restaurante', syllables: ['Res', 'tau', 'ran', 'te'] },
      { word: 'Cozinha', image: 'https://placehold.co/150x150/FFFACD/000000?text=Cozinha', syllables: ['Co', 'zi', 'nha'] },
      { word: 'Quarto', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Quarto', syllables: ['Quar', 'to'] },
      { word: 'Banheiro', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Banheiro', syllables: ['Ba', 'nhei', 'ro'] },
      { word: 'Sala', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Sala', syllables: ['Sa', 'la'] },
      { word: 'Jardim', image: 'https://placehold.co/150x150/98FB98/000000?text=Jardim', syllables: ['Jar', 'dim'] },
      { word: 'Flor', image: 'https://placehold.co/150x150/FF69B4/000000?text=Flor', syllables: ['Flor'] },
      { word: 'Folha', image: 'https://placehold.co/150x150/228B22/000000?text=Folha', syllables: ['Fo', 'lha'] },
      { word: 'Árvore', image: 'https://placehold.co/150x150/228B22/000000?text=Árvore', syllables: ['Ár', 'vo', 're'] },
      { word: 'Fruta', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Fruta', syllables: ['Fru', 'ta'] },
      { word: 'Maçã', image: 'https://placehold.co/150x150/FF0000/FFFFFF?text=Maçã', syllables: ['Ma', 'çã'] },
      { word: 'Banana', image: 'https://placehold.co/150x150/FFFF00/000000?text=Banana', syllables: ['Ba', 'na', 'na'] },
      { word: 'Uva', image: 'https://placehold.co/150x150/800080/FFFFFF?text=Uva', syllables: ['U', 'va'] },
      { word: 'Laranja', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Laranja', syllables: ['La', 'ran', 'ja'] },
      { word: 'Morango', image: 'https://placehold.co/150x150/FF0000/FFFFFF?text=Morango', syllables: ['Mo', 'ran', 'go'] },
      { word: 'Cenoura', image: 'https://placehold.co/150x150/FF8C00/000000?text=Cenoura', syllables: ['Ce', 'nou', 'ra'] },
      { word: 'Batata', image: 'https://placehold.co/150x150/CD853F/000000?text=Batata', syllables: ['Ba', 'ta', 'ta'] },
      { word: 'Tomate', image: 'https://placehold.co/150x150/FF6347/000000?text=Tomate', syllables: ['To', 'ma', 'te'] },
      { word: 'Alface', image: 'https://placehold.co/150x150/32CD32/000000?text=Alface', syllables: ['Al', 'fa', 'ce'] },
      { word: 'Pão', image: 'https://placehold.co/150x150/D2B48C/000000?text=Pão', syllables: ['Pão'] },
      { word: 'Queijo', image: 'https://placehold.co/150x150/F0E68C/000000?text=Queijo', syllables: ['Quei', 'jo'] },
      { word: 'Leite', image: 'https://placehold.co/150x150/F0F8FF/000000?text=Leite', syllables: ['Lei', 'te'] },
      { word: 'Ovo', image: 'https://placehold.co/150x150/FFFACD/000000?text=Ovo', syllables: ['O', 'vo'] },
      { word: 'Carne', image: 'https://placehold.co/150x150/CD5C5C/000000?text=Carne', syllables: ['Car', 'ne'] },
      { word: 'Arroz', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Arroz', syllables: ['Ar', 'roz'] },
      { word: 'Feijão', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Feijão', syllables: ['Fei', 'jão'] },
      { word: 'Sopa', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Sopa', syllables: ['So', 'pa'] },
      { word: 'Bolo', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Bolo', syllables: ['Bo', 'lo'] },
      { word: 'Doce', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Doce', syllables: ['Do', 'ce'] },
      { word: 'Chocolate', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Chocolate', syllables: ['Cho', 'co', 'la', 'te'] },
      { word: 'Sorvete', image: 'https://placehold.co/150x150/B0E0E6/000000?text=Sorvete', syllables: ['Sor', 've', 'te'] },
      { word: 'Suco', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Suco', syllables: ['Su', 'co'] },
      { word: 'Água', image: 'https://placehold.co/150x150/4682B4/000000?text=Água', syllables: ['Á', 'gua'] },
      { word: 'Copo', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Copo', syllables: ['Co', 'po'] },
      { word: 'Prato', image: 'https://placehold.co/150x150/D3D3D3/000000?text=Prato', syllables: ['Pra', 'to'] },
      { word: 'Colher', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Colher', syllables: ['Co', 'lher'] },
      { word: 'Garfo', image: 'https://placehold.co/150x150/708090/FFFFFF?text=Garfo', syllables: ['Gar', 'fo'] },
      { word: 'Faca', image: 'https://placehold.co/150x150/696969/FFFFFF?text=Faca', syllables: ['Fa', 'ca'] },
      { word: 'Cama', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Cama', syllables: ['Ca', 'ma'] },
      { word: 'Travesseiro', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Travesseiro', syllables: ['Tra', 'ves', 'sei', 'ro'] },
      { word: 'Cobertor', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Cobertor', syllables: ['Co', 'ber', 'tor'] },
      { word: 'Armário', image: 'https://placehold.co/150x150/CD853F/000000?text=Armário', syllables: ['Ar', 'má', 'rio'] },
      { word: 'Roupa', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Roupa', syllables: ['Rou', 'pa'] },
      { word: 'Sapato', image: 'https://placehold.co/150x150/CD853F/000000?text=Sapato', syllables: ['Sa', 'pa', 'to'] },
      { word: 'Meia', image: 'https://placehold.co/150x150/F0E68C/000000?text=Meia', syllables: ['Mei', 'a'] },
      { word: 'Chapéu', image: 'https://placehold.co/150x150/D2B48C/000000?text=Chapéu', syllables: ['Cha', 'péu'] },
      { word: 'Óculos', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Óculos', syllables: ['Ó', 'cu', 'los'] },
      { word: 'Relógio', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Relógio', syllables: ['Re', 'ló', 'gio'] },
      { word: 'Telefone', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Telefone', syllables: ['Te', 'le', 'fo', 'ne'] },
      { word: 'Televisão', image: 'https://placehold.co/150x150/6495ED/000000?text=Televisão', syllables: ['Te', 'le', 'vi', 'são'] },
      { word: 'Computador', image: 'https://placehold.co/150x150/4682B4/000000?text=Computador', syllables: ['Com', 'pu', 'ta', 'dor'] },
      { word: 'Rádio', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Rádio', syllables: ['Rá', 'dio'] },
      { word: 'Livro', image: 'https://placehold.co/150x150/D2B48C/000000?text=Livro', syllables: ['Li', 'vro'] },
      { word: 'Caderno', image: 'https://placehold.co/150x150/F4A460/000000?text=Caderno', syllables: ['Ca', 'der', 'no'] },
      { word: 'Lápis', image: 'https://placehold.co/150x150/FF6347/000000?text=Lápis', syllables: ['Lá', 'pis'] },
      { word: 'Caneta', image: 'https://placehold.co/150x150/98FB98/000000?text=Caneta', syllables: ['Ca', 'ne', 'ta'] },
      { word: 'Borracha', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Borracha', syllables: ['Bo', 'rra', 'cha'] },
      { word: 'Mochila', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Mochila', syllables: ['Mo', 'chi', 'la'] },
      { word: 'Tesoura', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Tesoura', syllables: ['Te', 'sou', 'ra'] },
      { word: 'Cola', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Cola', syllables: ['Co', 'la'] },
      { word: 'Tinta', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Tinta', syllables: ['Tin', 'ta'] },
      { word: 'Pincel', image: 'https://placehold.co/150x150/F0E68C/000000?text=Pincel', syllables: ['Pin', 'cel'] },
      { word: 'Papel', image: 'https://placehold.co/150x150/FFFACD/000000?text=Papel', syllables: ['Pa', 'pel'] },
      { word: 'Giz', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Giz', syllables: ['Giz'] },
      { word: 'Quadro', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Quadro', syllables: ['Qua', 'dro'] },
      { word: 'Música', image: 'https://placehold.co/150x150/DA70D6/000000?text=Música', syllables: ['Mú', 'si', 'ca'] },
      { word: 'Dança', image: 'https://placehold.co/150x150/FFB6C1/000000?text=Dança', syllables: ['Dan', 'ça'] },
      { word: 'Cantar', image: 'https://placehold.co/150x150/FFD700/000000?text=Cantar', syllables: ['Can', 'tar'] },
      { word: 'Brincar', image: 'https://placehold.co/150x150/87CEEB/000000?text=Brincar', syllables: ['Brin', 'car'] },
      { word: 'Correr', image: 'https://placehold.co/150x150/98FB98/000000?text=Correr', syllables: ['Cor', 'rer'] },
      { word: 'Pular', image: 'https://placehold.co/150x150/FF6347/000000?text=Pular', syllables: ['Pu', 'lar'] },
      { word: 'Andar', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Andar', syllables: ['An', 'dar'] },
      { word: 'Dormir', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Dormir', syllables: ['Dor', 'mir'] },
      { word: 'Acordar', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Acordar', syllables: ['A', 'cor', 'dar'] },
      { word: 'Comer', image: 'https://placehold.co/150x150/F0E68C/000000?text=Comer', syllables: ['Co', 'mer'] },
      { word: 'Beber', image: 'https://placehold.co/150x150/AFEEEE/000000?text=Beber', syllables: ['Be', 'ber'] },
      { word: 'Falar', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Falar', syllables: ['Fa', 'lar'] },
      { word: 'Ouvir', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Ouvir', syllables: ['Ou', 'vir'] },
      { word: 'Ver', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Ver', syllables: ['Ver'] },
      { word: 'Cheirar', image: 'https://placehold.co/150x150/FFFACD/000000?text=Cheirar', syllables: ['Chei', 'rar'] },
      { word: 'Tocar', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Tocar', syllables: ['To', 'car'] },
      { word: 'Pensar', image: 'https://placehold.co/150x150/F0E68C/000000?text=Pensar', syllables: ['Pen', 'sar'] },
      { word: 'Aprender', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Aprender', syllables: ['A', 'pren', 'der'] },
      { word: 'Ensinar', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Ensinar', syllables: ['En', 'si', 'nar'] },
      { word: 'Ler', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Ler', syllables: ['Ler'] },
      { word: 'Escrever', image: 'https://placehold.co/150x150/FFFACD/000000?text=Escrever', syllables: ['Es', 'cre', 'ver'] },
      { word: 'Desenhar', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Desenhar', syllables: ['De', 'se', 'nhar'] },
      { word: 'Pintar', image: 'https://placehold.co/150x150/F0E68C/000000?text=Pintar', syllables: ['Pin', 'tar'] },
      { word: 'Cortar', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Cortar', syllables: ['Cor', 'tar'] },
      { word: 'Colar', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Colar', syllables: ['Co', 'lar'] },
      { word: 'Abrir', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Abrir', syllables: ['A', 'brir'] },
      { word: 'Fechar', image: 'https://placehold.co/150x150/FFFACD/000000?text=Fechar', syllables: ['Fe', 'char'] },
      { word: 'Lavar', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Lavar', syllables: ['La', 'var'] },
      { word: 'Limpar', image: 'https://placehold.co/150x150/F0E68C/000000?text=Limpar', syllables: ['Lim', 'par'] },
      { word: 'Ajudar', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Ajudar', syllables: ['A', 'ju', 'dar'] },
      { word: 'Agradecer', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Agradecer', syllables: ['A', 'gra', 'de', 'cer'] },
      { word: 'Desculpar', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Desculpar', syllables: ['Des', 'cul', 'par'] },
      { word: 'Obrigado', image: 'https://placehold.co/150x150/FFFACD/000000?text=Obrigado', syllables: ['O', 'bri', 'ga', 'do'] },
      { word: 'Por favor', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Por favor', syllables: ['Por', 'fa', 'vor'] },
      { word: 'Bom dia', image: 'https://placehold.co/150x150/F0E68C/000000?text=Bom dia', syllables: ['Bom', 'di', 'a'] },
      { word: 'Boa tarde', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Boa tarde', syllables: ['Bo', 'a', 'tar', 'de'] },
      { word: 'Boa noite', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Boa noite', syllables: ['Bo', 'a', 'noi', 'te'] },
      { word: 'Até logo', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Até logo', syllables: ['A', 'té', 'lo', 'go'] },
      { word: 'Sim', image: 'https://placehold.co/150x150/FFFACD/000000?text=Sim', syllables: ['Sim'] },
      { word: 'Não', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Não', syllables: ['Não'] },
      { word: 'Amigo', image: 'https://placehold.co/150x150/F0E68C/000000?text=Amigo', syllables: ['A', 'mi', 'go'] },
      { word: 'Família', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Família', syllables: ['Fa', 'mí', 'lia'] },
      { word: 'Mãe', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Mãe', syllables: ['Mãe'] },
      { word: 'Pai', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Pai', syllables: ['Pai'] },
      { word: 'Irmão', image: 'https://placehold.co/150x150/FFFACD/000000?text=Irmão', syllables: ['Ir', 'mão'] },
      { word: 'Irmã', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Irmã', syllables: ['Ir', 'mã'] },
      { word: 'Avô', image: 'https://placehold.co/150x150/F0E68C/000000?text=Avô', syllables: ['A', 'vô'] },
      { word: 'Avó', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Avó', syllables: ['A', 'vó'] },
      { word: 'Bebê', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Bebê', syllables: ['Be', 'bê'] },
      { word: 'Criança', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Criança', syllables: ['Cri', 'an', 'ça'] },
      { word: 'Menino', image: 'https://placehold.co/150x150/FFFACD/000000?text=Menino', syllables: ['Me', 'ni', 'no'] },
      { word: 'Menina', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Menina', syllables: ['Me', 'ni', 'na'] },
      { word: 'Professor', image: 'https://placehold.co/150x150/F0E68C/000000?text=Professor', syllables: ['Pro', 'fes', 'sor'] },
      { word: 'Alunos', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Alunos', syllables: ['A', 'lu', 'nos'] },
    ],
  },
  en: {
    alphabet: [
      { letter: 'A', sound: 'Ayy', word: 'Apple', image: 'https://placehold.co/150x150/FF0000/FFFFFF?text=Apple', syllables: ['Ap', 'ple'] },
      { letter: 'B', sound: 'Bee', word: 'Ball', image: 'https://placehold.co/150x150/0000FF/FFFFFF?text=Ball', syllables: ['Ball'] },
      { letter: 'C', sound: 'See', word: 'Cat', image: 'https://placehold.co/150x150/800080/FFFFFF?text=Cat', syllables: ['Cat'] },
      { letter: 'D', sound: 'Dee', word: 'Dog', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Dog', syllables: ['Dog'] },
      { letter: 'E', sound: 'Eee', word: 'Elephant', image: 'https://placehold.co/150x150/008000/FFFFFF?text=Elephant', syllables: ['El', 'e', 'phant'] },
      { letter: 'F', sound: 'Eff', word: 'Fish', image: 'https://placehold.co/150x150/FFFF00/000000?text=Fish', syllables: ['Fish'] },
      { letter: 'G', sound: 'Gee', word: 'Grape', image: 'https://placehold.co/150x150/800080/FFFFFF?text=Grape', syllables: ['Grape'] },
      { letter: 'H', sound: 'Aitch', word: 'Hat', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Hat', syllables: ['Hat'] },
      { letter: 'I', sound: 'Eye', word: 'Ice', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Ice', syllables: ['Ice'] },
      { letter: 'J', sound: 'Jay', word: 'Jellyfish', image: 'https://placehold.co/150x150/F0E68C/000000?text=Jellyfish', syllables: ['Jel', 'ly', 'fish'] },
      { letter: 'K', sound: 'Kay', word: 'Kite', image: 'https://placehold.co/150x150/AFEEEE/000000?text=Kite', syllables: ['Kite'] },
      { letter: 'L', sound: 'Ell', word: 'Lion', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Lion', syllables: ['Li', 'on'] },
      { letter: 'M', sound: 'Em', word: 'Monkey', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Monkey', syllables: ['Mon', 'key'] },
      { letter: 'N', sound: 'En', word: 'Nest', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Nest', syllables: ['Nest'] },
      { letter: 'O', sound: 'Oh', word: 'Orange', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Orange', syllables: ['Or', 'ange'] },
      { letter: 'P', sound: 'Pee', word: 'Pig', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Pig', syllables: ['Pig'] },
      { letter: 'Q', sound: 'Cue', word: 'Queen', image: 'https://placehold.co/150x150/F0E68C/000000?text=Queen', syllables: ['Queen'] },
      { letter: 'R', sound: 'Arr', word: 'Rabbit', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Rabbit', syllables: ['Rab', 'bit'] },
      { letter: 'S', sound: 'Ess', word: 'Sun', image: 'https://placehold.co/150x150/FFD700/000000?text=Sun', syllables: ['Sun'] },
      { letter: 'T', sound: 'Tee', word: 'Tree', image: 'https://placehold.co/150x150/228B22/000000?text=Tree', syllables: ['Tree'] },
      { letter: 'U', sound: 'Yoo', word: 'Umbrella', image: 'https://placehold.co/150x150/DA70D6/000000?text=Umbrella', syllables: ['Um', 'brel', 'la'] },
      { letter: 'V', sound: 'Vee', word: 'Van', image: 'https://placehold.co/150x150/98FB98/000000?text=Van', syllables: ['Van'] },
      { letter: 'W', sound: 'Double-yoo', word: 'Water', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Water', syllables: ['Wa', 'ter'] },
      { letter: 'X', sound: 'Ex', word: 'Xylophone', image: 'https://placehold.co/150x150/FF6347/000000?text=Xylophone', syllables: ['Xyl', 'o', 'phone'] },
      { letter: 'Y', sound: 'Why', word: 'Yacht', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Yacht', syllables: ['Yacht'] },
      { letter: 'Z', sound: 'Zee', word: 'Zebra', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Zebra', syllables: ['Ze', 'bra'] },
    ],
    words: [
      { word: 'Cat', image: 'https://placehold.co/150x150/800080/FFFFFF?text=Cat', syllables: ['Cat'] },
      { word: 'Dog', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Dog', syllables: ['Dog'] },
      { word: 'House', image: 'https://placehold.co/150x150/FF6347/000000?text=House', syllables: ['House'] },
      { word: 'Book', image: 'https://placehold.co/150x150/D2B48C/000000?text=Book', syllables: ['Book'] },
      { word: 'Chair', image: 'https://placehold.co/150x150/A0522D/000000?text=Chair', syllables: ['Chair'] },
      { word: 'Table', image: 'https://placehold.co/150x150/F4A460/000000?text=Table', syllables: ['Ta', 'ble'] },
      { word: 'Bird', image: 'https://placehold.co/150x150/B0E0E6/000000?text=Bird', syllables: ['Bird'] },
      { word: 'Flower', image: 'https://placehold.co/150x150/FF69B4/000000?text=Flower', syllables: ['Flow', 'er'] },
      { word: 'Car', image: 'https://placehold.co/150x150/FF4500/000000?text=Car', syllables: ['Car'] },
      { word: 'Tree', image: 'https://placehold.co/150x150/228B22/000000?text=Tree', syllables: ['Tree'] },
      { word: 'Sun', image: 'https://placehold.co/150x150/FFD700/000000?text=Sun', syllables: ['Sun'] },
      { word: 'Moon', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Moon', syllables: ['Moon'] },
      { word: 'Star', image: 'https://placehold.co/150x150/FFFF00/000000?text=Star', syllables: ['Star'] },
      { word: 'Cloud', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Cloud', syllables: ['Cloud'] },
      { word: 'Rain', image: 'https://placehold.co/150x150/6495ED/000000?text=Rain', syllables: ['Rain'] },
      { word: 'Wind', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Wind', syllables: ['Wind'] },
      { word: 'Fire', image: 'https://placehold.co/150x150/FF4500/000000?text=Fire', syllables: ['Fire'] },
      { word: 'Water', image: 'https://placehold.co/150x150/4682B4/000000?text=Water', syllables: ['Wa', 'ter'] },
      { word: 'Earth', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Earth', syllables: ['Earth'] },
      { word: 'Mountain', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Mountain', syllables: ['Moun', 'tain'] },
      { word: 'River', image: 'https://placehold.co/150x150/00BFFF/000000?text=River', syllables: ['Riv', 'er'] },
      { word: 'Sea', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Sea', syllables: ['Sea'] },
      { word: 'Beach', image: 'https://placehold.co/150x150/F4A460/000000?text=Beach', syllables: ['Beach'] },
      { word: 'Sand', image: 'https://placehold.co/150x150/F0E68C/000000?text=Sand', syllables: ['Sand'] },
      { word: 'Fish', image: 'https://placehold.co/150x150/FF6347/000000?text=Fish', syllables: ['Fish'] },
      { word: 'Butterfly', image: 'https://placehold.co/150x150/FFB6C1/000000?text=Butterfly', syllables: ['But', 'ter', 'fly'] },
      { word: 'Bee', image: 'https://placehold.co/150x150/FFD700/000000?text=Bee', syllables: ['Bee'] },
      { word: 'Ant', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Ant', syllables: ['Ant'] },
      { word: 'Spider', image: 'https://placehold.co/150x150/696969/FFFFFF?text=Spider', syllables: ['Spi', 'der'] },
      { word: 'Snake', image: 'https://placehold.co/150x150/32CD32/000000?text=Snake', syllables: ['Snake'] },
      { word: 'Lion', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Lion', syllables: ['Li', 'on'] },
      { word: 'Tiger', image: 'https://placehold.co/150x150/FF8C00/000000?text=Tiger', syllables: ['Ti', 'ger'] },
      { word: 'Elephant', image: 'https://placehold.co/150x150/D3D3D3/000000?text=Elephant', syllables: ['El', 'e', 'phant'] },
      { word: 'Giraffe', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Giraffe', syllables: ['Gi', 'raffe'] },
      { word: 'Zebra', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Zebra', syllables: ['Ze', 'bra'] },
      { word: 'Monkey', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Monkey', syllables: ['Mon', 'key'] },
      { word: 'Bear', image: 'https://placehold.co/150x150/A0522D/000000?text=Bear', syllables: ['Bear'] },
      { word: 'Rabbit', image: 'https://placehold.co/150x150/F0F8FF/000000?text=Rabbit', syllables: ['Rab', 'bit'] },
      { word: 'Fox', image: 'https://placehold.co/150x150/CD5C5C/000000?text=Fox', syllables: ['Fox'] },
      { word: 'Wolf', image: 'https://placehold.co/150x150/708090/FFFFFF?text=Wolf', syllables: ['Wolf'] },
      { word: 'Deer', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Deer', syllables: ['Deer'] },
      { word: 'Owl', image: 'https://placehold.co/150x150/D2B48C/000000?text=Owl', syllables: ['Owl'] },
      { word: 'Eagle', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Eagle', syllables: ['Ea', 'gle'] },
      { word: 'Duck', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Duck', syllables: ['Duck'] },
      { word: 'Chicken', image: 'https://placehold.co/150x150/F0E68C/000000?text=Chicken', syllables: ['Chick', 'en'] },
      { word: 'Rooster', image: 'https://placehold.co/150x150/FF6347/000000?text=Rooster', syllables: ['Roos', 'ter'] },
      { word: 'Sheep', image: 'https://placehold.co/150x150/FFFACD/000000?text=Sheep', syllables: ['Sheep'] },
      { word: 'Pig', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Pig', syllables: ['Pig'] },
      { word: 'Horse', image: 'https://placehold.co/150x150/CD853F/000000?text=Horse', syllables: ['Horse'] },
      { word: 'Cow', image: 'https://placehold.co/150x150/98FB98/000000?text=Cow', syllables: ['Cow'] },
      { word: 'Goat', image: 'https://placehold.co/150x150/D2B48C/000000?text=Goat', syllables: ['Goat'] },
      { word: 'Doll', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Doll', syllables: ['Doll'] },
      { word: 'Bicycle', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Bicycle', syllables: ['Bi', 'cy', 'cle'] },
      { word: 'Airplane', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Airplane', syllables: ['Air', 'plane'] },
      { word: 'Train', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Train', syllables: ['Train'] },
      { word: 'Ship', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Ship', syllables: ['Ship'] },
      { word: 'Boat', image: 'https://placehold.co/150x150/4682B4/000000?text=Boat', syllables: ['Boat'] },
      { word: 'Rocket', image: 'https://placehold.co/150x150/8A2BE2/FFFFFF?text=Rocket', syllables: ['Rock', 'et'] },
      { word: 'Building', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Building', syllables: ['Build', 'ing'] },
      { word: 'School', image: 'https://placehold.co/150x150/98FB98/000000?text=School', syllables: ['School'] },
      { word: 'Hospital', image: 'https://placehold.co/150x150/DC143C/FFFFFF?text=Hospital', syllables: ['Hos', 'pi', 'tal'] },
      { word: 'Park', image: 'https://placehold.co/150x150/32CD32/000000?text=Park', syllables: ['Park'] },
      { word: 'Store', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Store', syllables: ['Store'] },
      { word: 'Market', image: 'https://placehold.co/150x150/F0E68C/000000?text=Market', syllables: ['Mar', 'ket'] },
      { word: 'Restaurant', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Restaurant', syllables: ['Res', 'tau', 'rant'] },
      { word: 'Kitchen', image: 'https://placehold.co/150x150/FFFACD/000000?text=Kitchen', syllables: ['Kitch', 'en'] },
      { word: 'Bedroom', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Bedroom', syllables: ['Bed', 'room'] },
      { word: 'Bathroom', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Bathroom', syllables: ['Bath', 'room'] },
      { word: 'Living Room', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Living Room', syllables: ['Liv', 'ing', 'Room'] },
      { word: 'Garden', image: 'https://placehold.co/150x150/98FB98/000000?text=Garden', syllables: ['Gar', 'den'] },
      { word: 'Leaf', image: 'https://placehold.co/150x150/228B22/000000?text=Leaf', syllables: ['Leaf'] },
      { word: 'Fruit', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Fruit', syllables: ['Fruit'] },
      { word: 'Apple', image: 'https://placehold.co/150x150/FF0000/FFFFFF?text=Apple', syllables: ['Ap', 'ple'] },
      { word: 'Banana', image: 'https://placehold.co/150x150/FFFF00/000000?text=Banana', syllables: ['Ba', 'na', 'na'] },
      { word: 'Grape', image: 'https://placehold.co/150x150/800080/FFFFFF?text=Grape', syllables: ['Grape'] },
      { word: 'Orange', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Orange', syllables: ['Or', 'ange'] },
      { word: 'Strawberry', image: 'https://placehold.co/150x150/FF0000/FFFFFF?text=Strawberry', syllables: ['Straw', 'ber', 'ry'] },
      { word: 'Carrot', image: 'https://placehold.co/150x150/FF8C00/000000?text=Carrot', syllables: ['Car', 'rot'] },
      { word: 'Potato', image: 'https://placehold.co/150x150/CD853F/000000?text=Potato', syllables: ['Po', 'ta', 'to'] },
      { word: 'Tomato', image: 'https://placehold.co/150x150/FF6347/000000?text=Tomato', syllables: ['To', 'ma', 'to'] },
      { word: 'Lettuce', image: 'https://placehold.co/150x150/32CD32/000000?text=Lettuce', syllables: ['Let', 'tuce'] },
      { word: 'Bread', image: 'https://placehold.co/150x150/D2B48C/000000?text=Bread', syllables: ['Bread'] },
      { word: 'Cheese', image: 'https://placehold.co/150x150/F0E68C/000000?text=Cheese', syllables: ['Cheese'] },
      { word: 'Milk', image: 'https://placehold.co/150x150/F0F8FF/000000?text=Milk', syllables: ['Milk'] },
      { word: 'Egg', image: 'https://placehold.co/150x150/FFFACD/000000?text=Egg', syllables: ['Egg'] },
      { word: 'Meat', image: 'https://placehold.co/150x150/CD5C5C/000000?text=Meat', syllables: ['Meat'] },
      { word: 'Rice', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Rice', syllables: ['Rice'] },
      { word: 'Beans', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Beans', syllables: ['Beans'] },
      { word: 'Soup', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Soup', syllables: ['Soup'] },
      { word: 'Cake', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Cake', syllables: ['Cake'] },
      { word: 'Candy', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Candy', syllables: ['Can', 'dy'] },
      { word: 'Chocolate', image: 'https://placehold.co/150x150/8B4513/FFFFFF?text=Chocolate', syllables: ['Choc', 'o', 'late'] },
      { word: 'Ice Cream', image: 'https://placehold.co/150x150/B0E0E6/000000?text=Ice Cream', syllables: ['Ice', 'Cream'] },
      { word: 'Juice', image: 'https://placehold.co/150x150/FFA500/FFFFFF?text=Juice', syllables: ['Juice'] },
      { word: 'Cup', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Cup', syllables: ['Cup'] },
      { word: 'Plate', image: 'https://placehold.co/150x150/D3D3D3/000000?text=Plate', syllables: ['Plate'] },
      { word: 'Spoon', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Spoon', syllables: ['Spoon'] },
      { word: 'Fork', image: 'https://placehold.co/150x150/708090/FFFFFF?text=Fork', syllables: ['Fork'] },
      { word: 'Knife', image: 'https://placehold.co/150x150/696969/FFFFFF?text=Knife', syllables: ['Knife'] },
      { word: 'Bed', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Bed', syllables: ['Bed'] },
      { word: 'Pillow', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Pillow', syllables: ['Pil', 'low'] },
      { word: 'Blanket', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Blanket', syllables: ['Blan', 'ket'] },
      { word: 'Closet', image: 'https://placehold.co/150x150/CD853F/000000?text=Closet', syllables: ['Clos', 'et'] },
      { word: 'Clothes', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Clothes', syllables: ['Clothes'] },
      { word: 'Sock', image: 'https://placehold.co/150x150/F0E68C/000000?text=Sock', syllables: ['Sock'] },
      { word: 'Hat', image: 'https://placehold.co/150x150/D2B48C/000000?text=Hat', syllables: ['Hat'] },
      { word: 'Glasses', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Glasses', syllables: ['Glass', 'es'] },
      { word: 'Watch', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Watch', syllables: ['Watch'] },
      { word: 'Phone', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Phone', syllables: ['Phone'] },
      { word: 'Television', image: 'https://placehold.co/150x150/6495ED/000000?text=Television', syllables: ['Tel', 'e', 'vi', 'sion'] },
      { word: 'Computer', image: 'https://placehold.co/150x150/4682B4/000000?text=Computer', syllables: ['Com', 'pu', 'ter'] },
      { word: 'Radio', image: 'https://placehold.co/150x150/1E90FF/FFFFFF?text=Radio', syllables: ['Ra', 'dio'] },
      { word: 'Notebook', image: 'https://placehold.co/150x150/F4A460/000000?text=Notebook', syllables: ['Note', 'book'] },
      { word: 'Pencil', image: 'https://placehold.co/150x150/FF6347/000000?text=Pencil', syllables: ['Pen', 'cil'] },
      { word: 'Pen', image: 'https://placehold.co/150x150/98FB98/000000?text=Pen', syllables: ['Pen'] },
      { word: 'Eraser', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Eraser', syllables: ['E', 'ra', 'ser'] },
      { word: 'Backpack', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Backpack', syllables: ['Back', 'pack'] },
      { word: 'Scissors', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Scissors', syllables: ['Scis', 'sors'] },
      { word: 'Glue', image: 'https://placehold.co/150x150/E0FFFF/000000?text=Glue', syllables: ['Glue'] },
      { word: 'Paint', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Paint', syllables: ['Paint'] },
      { word: 'Brush', image: 'https://placehold.co/150x150/F0E68C/000000?text=Brush', syllables: ['Brush'] },
      { word: 'Paper', image: 'https://placehold.co/150x150/FFFACD/000000?text=Paper', syllables: ['Pa', 'per'] },
      { word: 'Chalk', image: 'https://placehold.co/150x150/C0C0C0/000000?text=Chalk', syllables: ['Chalk'] },
      { word: 'Board', image: 'https://placehold.co/150x150/A9A9A9/000000?text=Board', syllables: ['Board'] },
      { word: 'Music', image: 'https://placehold.co/150x150/DA70D6/000000?text=Music', syllables: ['Mu', 'sic'] },
      { word: 'Dance', image: 'https://placehold.co/150x150/FFB6C1/000000?text=Dance', syllables: ['Dance'] },
      { word: 'Sing', image: 'https://placehold.co/150x150/FFD700/000000?text=Sing', syllables: ['Sing'] },
      { word: 'Play', image: 'https://placehold.co/150x150/87CEEB/000000?text=Play', syllables: ['Play'] },
      { word: 'Run', image: 'https://placehold.co/150x150/98FB98/000000?text=Run', syllables: ['Run'] },
      { word: 'Jump', image: 'https://placehold.co/150x150/FF6347/000000?text=Jump', syllables: ['Jump'] },
      { word: 'Walk', image: 'https://placehold.co/150x150/ADD8E6/000000?text=Walk', syllables: ['Walk'] },
      { word: 'Sleep', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Sleep', syllables: ['Sleep'] },
      { word: 'Wake up', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Wake up', syllables: ['Wake', 'up'] },
      { word: 'Eat', image: 'https://placehold.co/150x150/F0E68C/000000?text=Eat', syllables: ['Eat'] },
      { word: 'Drink', image: 'https://placehold.co/150x150/AFEEEE/000000?text=Drink', syllables: ['Drink'] },
      { word: 'Talk', image: 'https://placehold.co/150x150/F5DEB3/000000?text=Talk', syllables: ['Talk'] },
      { word: 'Listen', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Listen', syllables: ['Lis', 'ten'] },
      { word: 'See', image: 'https://placehold.co/150x150/B0C4DE/000000?text=See', syllables: ['See'] },
      { word: 'Smell', image: 'https://placehold.co/150x150/FFFACD/000000?text=Smell', syllables: ['Smell'] },
      { word: 'Touch', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Touch', syllables: ['Touch'] },
      { word: 'Think', image: 'https://placehold.co/150x150/F0E68C/000000?text=Think', syllables: ['Think'] },
      { word: 'Learn', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Learn', syllables: ['Learn'] },
      { word: 'Teach', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Teach', syllables: ['Teach'] },
      { word: 'Read', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Read', syllables: ['Read'] },
      { word: 'Write', image: 'https://placehold.co/150x150/FFFACD/000000?text=Write', syllables: ['Write'] },
      { word: 'Draw', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Draw', syllables: ['Draw'] },
      { word: 'Color', image: 'https://placehold.co/150x150/F0E68C/000000?text=Color', syllables: ['Col', 'or'] },
      { word: 'Cut', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Cut', syllables: ['Cut'] },
      { word: 'Glue', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Glue', syllables: ['Glue'] },
      { word: 'Open', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Open', syllables: ['O', 'pen'] },
      { word: 'Close', image: 'https://placehold.co/150x150/FFFACD/000000?text=Close', syllables: ['Close'] },
      { word: 'Wash', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Wash', syllables: ['Wash'] },
      { word: 'Clean', image: 'https://placehold.co/150x150/F0E68C/000000?text=Clean', syllables: ['Clean'] },
      { word: 'Help', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Help', syllables: ['Help'] },
      { word: 'Thank you', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Thank you', syllables: ['Thank', 'you'] },
      { word: 'Excuse me', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Excuse me', syllables: ['Ex', 'cuse', 'me'] },
      { word: 'Please', image: 'https://placehold.co/150x150/FFFACD/000000?text=Please', syllables: ['Please'] },
      { word: 'Good morning', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Good morning', syllables: ['Good', 'morn', 'ing'] },
      { word: 'Good afternoon', image: 'https://placehold.co/150x150/F0E68C/000000?text=Good afternoon', syllables: ['Good', 'af', 'ter', 'noon'] },
      { word: 'Good night', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Good night', syllables: ['Good', 'night'] },
      { word: 'Goodbye', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Goodbye', syllables: ['Good', 'bye'] },
      { word: 'Yes', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Yes', syllables: ['Yes'] },
      { word: 'No', image: 'https://placehold.co/150x150/FFFACD/000000?text=No', syllables: ['No'] },
      { word: 'Friend', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Friend', syllables: ['Friend'] },
      { word: 'Family', image: 'https://placehold.co/150x150/F0E68C/000000?text=Family', syllables: ['Fam', 'i', 'ly'] },
      { word: 'Mother', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Mother', syllables: ['Moth', 'er'] },
      { word: 'Father', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Father', syllables: ['Fa', 'ther'] },
      { word: 'Brother', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Brother', syllables: ['Broth', 'er'] },
      { word: 'Sister', image: 'https://placehold.co/150x150/FFFACD/000000?text=Sister', syllables: ['Sis', 'ter'] },
      { word: 'Grandfather', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Grandfather', syllables: ['Grand', 'fa', 'ther'] },
      { word: 'Grandmother', image: 'https://placehold.co/150x150/F0E68C/000000?text=Grandmother', syllables: ['Grand', 'moth', 'er'] },
      { word: 'Baby', image: 'https://placehold.co/150x150/FFC0CB/000000?text=Baby', syllables: ['Ba', 'by'] },
      { word: 'Child', image: 'https://placehold.co/150x150/DDA0DD/000000?text=Child', syllables: ['Child'] },
      { word: 'Boy', image: 'https://placehold.co/150x150/B0C4DE/000000?text=Boy', syllables: ['Boy'] },
      { word: 'Girl', image: 'https://placehold.co/150x150/FFFACD/000000?text=Girl', syllables: ['Girl'] },
      { word: 'Teacher', image: 'https://placehold.co/150x150/FFDAB9/000000?text=Teacher', syllables: ['Teach', 'er'] },
      { word: 'Students', image: 'https://placehold.co/150x150/F0E68C/000000?text=Students', syllables: ['Stu', 'dents'] },
    ],
  },
};

// Utility function for text-to-speech
const speak = (text, lang) => {
  if ('speechSynthesis' in window) {
    const utterance = new SpeechSynthesisUtterance(text);
    utterance.lang = lang === 'pt' ? 'pt-BR' : 'en-US';
    window.speechSynthesis.speak(utterance);
  } else {
    console.warn('SpeechSynthesis API not supported in this browser.');
  }
};

// Alphabet Learning Component
const AlphabetLearning = ({ currentLang }) => {
  const alphabet = wordsData[currentLang].alphabet;
  const [currentIndex, setCurrentIndex] = useState(0);
  const currentItem = alphabet[currentIndex];

  // Effect to speak letter/word when current item changes
  useEffect(() => {
    if (currentItem) {
      speak(currentItem.letter, currentLang);
      setTimeout(() => speak(currentItem.word, currentLang), 1000); // Speak word after a slight delay
    }
  }, [currentItem, currentLang]);

  const handleNext = () => {
    setCurrentIndex((prevIndex) => (prevIndex + 1) % alphabet.length);
  };

  const handlePrev = () => {
    setCurrentIndex((prevIndex) => (prevIndex - 1 + alphabet.length) % alphabet.length);
  };

  const handleRepeatSound = () => {
    if (currentItem) {
      speak(currentItem.sound, currentLang);
      setTimeout(() => speak(currentItem.word, currentLang), 1000);
    }
  };

  return (
    <div className="flex flex-col items-center p-6 bg-blue-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto">
      <h2 className="text-3xl font-bold text-blue-800 mb-6">
        {currentLang === 'pt' ? 'Aprender o Alfabeto' : 'Learn the Alphabet'}
      </h2>
      {currentItem && (
        <div className="flex flex-col items-center space-y-4">
          <div className="text-9xl font-extrabold text-blue-600 mb-4">{currentItem.letter}</div>
          <button
            onClick={handleRepeatSound}
            className="flex items-center justify-center p-3 bg-blue-500 text-white rounded-full shadow-md hover:bg-blue-600 transition-colors"
          >
            <Volume2 size={32} />
            <span className="ml-2 text-xl">{currentLang === 'pt' ? 'Ouvir Som' : 'Hear Sound'}</span>
          </button>
          <div className="flex flex-col items-center mt-6">
            <p className="text-4xl font-semibold text-blue-700">{currentItem.word}</p>
            <img
              src={currentItem.image}
              alt={currentItem.word}
              className="w-48 h-48 object-contain rounded-lg shadow-md mt-4"
              onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/150x150/CCCCCC/000000?text=${currentItem.word}`; }}
            />
          </div>
          <div className="flex space-x-4 mt-8">
            <button
              onClick={handlePrev}
              className="p-3 bg-blue-500 text-white rounded-full shadow-md hover:bg-blue-600 transition-colors"
            >
              <ChevronLeft size={28} />
            </button>
            <button
              onClick={handleNext}
              className="p-3 bg-blue-500 text-white rounded-full shadow-md hover:bg-blue-600 transition-colors"
            >
              <ChevronRight size={28} />
            </button>
          </div>
        </div>
      )}
    </div>
  );
};

// Activity: Drag Letter to Correct Image
const DragLetterActivity = ({ currentLang }) => {
  const alphabet = wordsData[currentLang].alphabet;
  const [currentWord, setCurrentWord] = useState(null);
  const [shuffledLetters, setShuffledLetters] = useState([]);
  const [feedback, setFeedback] = useState('');

  const generateNewActivity = useCallback(() => {
    const randomWord = alphabet[Math.floor(Math.random() * alphabet.length)];
    setCurrentWord(randomWord);
    const letters = randomWord.word.split('').sort(() => 0.5 - Math.random());
    setShuffledLetters(letters.map((char, index) => ({ id: `${char}-${index}`, char })));
    setFeedback('');
  }, [alphabet]);

  useEffect(() => {
    generateNewActivity();
  }, [generateNewActivity]);

  const handleDragStart = (e, letter) => {
    e.dataTransfer.setData('letter', letter);
  };

  const handleDrop = (e) => {
    e.preventDefault();
    const droppedLetter = e.dataTransfer.getData('letter');
    if (droppedLetter.toLowerCase() === currentWord.letter.toLowerCase()) {
      setFeedback(currentLang === 'pt' ? 'Correto!' : 'Correct!');
      speak(currentLang === 'pt' ? 'Correto!' : 'Correct!', currentLang);
      setTimeout(generateNewActivity, 1500);
    } else {
      setFeedback(currentLang === 'pt' ? 'Tente novamente!' : 'Try again!');
      speak(currentLang === 'pt' ? 'Tente novamente!' : 'Try again!', currentLang);
    }
  };

  const handleDragOver = (e) => {
    e.preventDefault();
  };

  if (!currentWord) return null;

  return (
    <div className="flex flex-col items-center p-6 bg-green-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto mt-8">
      <h3 className="text-2xl font-bold text-green-800 mb-4">
        {currentLang === 'pt' ? 'Arrastar a Letra até a Imagem' : 'Drag Letter to Image'}
      </h3>
      <div className="mb-6">
        <img
          src={currentWord.image}
          alt={currentWord.word}
          className="w-48 h-48 object-contain rounded-lg shadow-md"
          onDrop={handleDrop}
          onDragOver={handleDragOver}
          onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/150x150/CCCCCC/000000?text=${currentWord.word}`; }}
        />
        <p className="text-xl text-green-700 mt-2">{currentWord.word}</p>
      </div>
      <div className="flex flex-wrap justify-center gap-4 mb-4">
        {shuffledLetters.map((item) => (
          <div
            key={item.id}
            draggable
            onDragStart={(e) => handleDragStart(e, item.char)}
            className="cursor-grab text-5xl font-bold p-4 bg-green-300 rounded-lg shadow-md hover:bg-green-400 transition-colors"
            onClick={() => speak(item.char, currentLang)} // Speak letter on click
          >
            {item.char.toUpperCase()}
          </div>
        ))}
      </div>
      {feedback && (
        <p className={`text-2xl font-semibold ${feedback.includes('Correto') || feedback.includes('Correct') ? 'text-green-700' : 'text-red-600'}`}>
          {feedback}
        </p>
      )}
    </div>
  );
};

// Activity: Color the Letter
const ColorLetterActivity = ({ currentLang }) => {
  const alphabet = wordsData[currentLang].alphabet;
  const words = wordsData[currentLang].words;

  const [paintingMode, setPaintingMode] = useState('letter'); // 'letter' or 'word'
  const [currentText, setCurrentText] = useState(alphabet[0].letter); // Initialize with first letter

  const canvasRef = useRef(null);
  const isDrawing = useRef(false);

  // Update currentText when paintingMode or currentLang changes
  useEffect(() => {
    if (paintingMode === 'letter') {
      setCurrentText(alphabet[0]?.letter || '');
    } else {
      setCurrentText(words[0]?.word || '');
    }
  }, [paintingMode, currentLang, alphabet, words]);

  const drawLetter = useCallback(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear canvas

    // Set canvas dimensions based on parent size
    const parent = canvas.parentElement;
    canvas.width = parent.clientWidth;
    canvas.height = parent.clientHeight;

    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillStyle = '#ccc'; // Outline color

    let fontSize = Math.min(canvas.width, canvas.height) * 0.7; // Start with a large font size
    ctx.font = `${fontSize}px Arial`;
    let textToDraw = currentText;

    // Adjust font size for words to fit within canvas width
    if (paintingMode === 'word') {
      while (ctx.measureText(textToDraw).width > canvas.width * 0.9 && fontSize > 10) {
        fontSize -= 5;
        ctx.font = `${fontSize}px Arial`;
      }
    } else { // For letters, ensure it's large enough
        fontSize = Math.min(canvas.width, canvas.height) * 0.7;
        ctx.font = `${fontSize}px Arial`;
    }

    ctx.fillText(textToDraw, canvas.width / 2, canvas.height / 2);
  }, [currentText, paintingMode]); // Added paintingMode to dependencies

  useEffect(() => {
    drawLetter();
  }, [currentText, drawLetter]);

  const startDrawing = (e) => {
    isDrawing.current = true;
    draw(e);
  };

  const stopDrawing = () => {
    isDrawing.current = false;
    const canvas = canvasRef.current;
    if (canvas) {
      const ctx = canvas.getContext('2d');
      ctx.beginPath(); // Reset path
    }
  };

  const draw = (e) => {
    if (!isDrawing.current) return;
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    const rect = canvas.getBoundingClientRect();
    const x = (e.clientX || e.touches[0].clientX) - rect.left;
    const y = (e.clientY || e.touches[0].clientY) - rect.top;

    ctx.lineWidth = 20;
    ctx.lineCap = 'round';
    ctx.strokeStyle = '#FF0000'; // Red color for drawing

    ctx.lineTo(x, y);
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(x, y);
  };

  const clearCanvas = () => {
    const canvas = canvasRef.current;
    if (canvas) {
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawLetter(); // Redraw the letter/word outline
    }
  };

  const handleTextChange = (e) => {
    setCurrentText(e.target.value);
  };

  const handlePaintingModeChange = (mode) => {
    setPaintingMode(mode);
  };

  const availableTexts = paintingMode === 'letter' ? alphabet : words;

  return (
    <div className="flex flex-col items-center p-6 bg-purple-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto mt-8">
      <h3 className="text-2xl font-bold text-purple-800 mb-4">
        {currentLang === 'pt' ? 'Pintar a Letra ou Palavra' : 'Color the Letter or Word'}
      </h3>
      <div className="flex space-x-4 mb-4">
        <button
          onClick={() => handlePaintingModeChange('letter')}
          className={`px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
            paintingMode === 'letter'
              ? 'bg-purple-600 text-white'
              : 'bg-purple-400 text-white hover:bg-purple-500'
          }`}
        >
          {currentLang === 'pt' ? 'Letra' : 'Letter'}
        </button>
        <button
          onClick={() => handlePaintingModeChange('word')}
          className={`px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
            paintingMode === 'word'
              ? 'bg-purple-600 text-white'
              : 'bg-purple-400 text-white hover:bg-purple-500'
          }`}
        >
          {currentLang === 'pt' ? 'Palavra' : 'Word'}
        </button>
      </div>
      <select
        value={currentText}
        onChange={handleTextChange}
        className="p-2 mb-4 rounded-md border-2 border-purple-400 focus:outline-none focus:ring-2 focus:ring-purple-500"
      >
        {availableTexts.map((item, index) => (
          <option key={index} value={paintingMode === 'letter' ? item.letter : item.word}>
            {paintingMode === 'letter' ? item.letter : item.word}
          </option>
        ))}
      </select>
      <div className="relative w-full h-64 border-2 border-purple-400 rounded-lg overflow-hidden bg-white">
        <canvas
          ref={canvasRef}
          className="absolute top-0 left-0 w-full h-full"
          onMouseDown={startDrawing}
          onMouseUp={stopDrawing}
          onMouseLeave={stopDrawing}
          onMouseMove={draw}
          onTouchStart={startDrawing}
          onTouchEnd={stopDrawing}
          onTouchCancel={stopDrawing}
          onTouchMove={draw}
        ></canvas>
      </div>
      <button
        onClick={clearCanvas}
        className="mt-4 p-3 bg-purple-500 text-white rounded-full shadow-md hover:bg-purple-600 transition-colors flex items-center"
      >
        <Paintbrush size={24} className="mr-2" />
        {currentLang === 'pt' ? 'Limpar' : 'Clear'}
      </button>
    </div>
  );
};

// Word Formation Component
const WordFormation = ({ currentLang }) => {
  const words = wordsData[currentLang].words;
  const [currentWord, setCurrentWord] = useState(null);
  const [shuffledLetters, setShuffledLetters] = useState([]);
  const [placedLetters, setPlacedLetters] = useState([]);
  const [feedback, setFeedback] = useState('');

  const generateNewWord = useCallback(() => {
    const randomWord = words[Math.floor(Math.random() * words.length)];
    setCurrentWord(randomWord);
    const letters = randomWord.word.split('').sort(() => 0.5 - Math.random());
    setShuffledLetters(letters.map((char, index) => ({ id: `${char}-${index}`, char })));
    setPlacedLetters(Array(randomWord.word.length).fill(null));
    setFeedback('');
  }, [words]);

  useEffect(() => {
    generateNewWord();
  }, [generateNewWord]);

  const handleLetterClick = (letterObj, originalIndex) => {
    speak(letterObj.char, currentLang); // Speak the letter when clicked

    // Find the first empty slot
    const emptyIndex = placedLetters.findIndex(slot => slot === null);
    if (emptyIndex !== -1) {
      const newPlacedLetters = [...placedLetters];
      newPlacedLetters[emptyIndex] = letterObj.char;
      setPlacedLetters(newPlacedLetters);

      // Remove from shuffled letters
      const newShuffledLetters = shuffledLetters.filter(item => item.id !== letterObj.id);
      setShuffledLetters(newShuffledLetters);

      // Check if word is complete
      if (newPlacedLetters.every(slot => slot !== null)) {
        const formedWord = newPlacedLetters.join('');
        if (formedWord.toLowerCase() === currentWord.word.toLowerCase()) {
          setFeedback(currentLang === 'pt' ? 'Parabéns! Você formou a palavra!' : 'Congratulations! You formed the word!');
          speak(currentWord.word, currentLang);
          setTimeout(generateNewWord, 2000);
        } else {
          setFeedback(currentLang === 'pt' ? 'Ops! Tente novamente.' : 'Oops! Try again.');
          setTimeout(() => {
            setPlacedLetters(Array(currentWord.word.length).fill(null));
            setShuffledLetters(currentWord.word.split('').sort(() => 0.5 - Math.random()).map((char, index) => ({ id: `${char}-${index}`, char })));
            setFeedback('');
          }, 1500);
        }
      }
    }
  };

  if (!currentWord) return null;

  return (
    <div className="flex flex-col items-center p-6 bg-yellow-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto">
      <h2 className="text-3xl font-bold text-yellow-800 mb-6">
        {currentLang === 'pt' ? 'Formar Palavras' : 'Word Formation'}
      </h2>
      <div className="mb-6 flex flex-col items-center">
        <img
          src={currentWord.image}
          alt={currentWord.word}
          className="w-48 h-48 object-contain rounded-lg shadow-md mb-4"
          onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/150x150/CCCCCC/000000?text=${currentWord.word}`; }}
        />
        <p className="text-xl text-yellow-700">
          {currentLang === 'pt' ? 'Forme a palavra para esta imagem:' : 'Form the word for this image:'}
        </p>
      </div>

      <div className="flex justify-center gap-2 mb-6 p-4 bg-yellow-200 rounded-lg shadow-inner">
        {placedLetters.map((letter, index) => (
          <div
            key={index}
            className="w-12 h-12 flex items-center justify-center border-2 border-yellow-500 rounded-md text-3xl font-bold text-yellow-800 bg-white"
          >
            {letter}
          </div>
        ))}
      </div>

      <div className="flex flex-wrap justify-center gap-3 mb-6">
        {shuffledLetters.map((item) => (
          <button
            key={item.id}
            onClick={() => handleLetterClick(item)}
            className="p-4 bg-yellow-400 text-white rounded-lg shadow-md hover:bg-yellow-500 transition-colors text-3xl font-bold"
          >
            {item.char.toUpperCase()}
          </button>
        ))}
      </div>
      {feedback && (
        <p className={`text-2xl font-semibold ${feedback.includes('Parabéns') || feedback.includes('Congratulations') ? 'text-green-700' : 'text-red-600'}`}>
          {feedback}
        </p>
      )}
      <button
        onClick={generateNewWord}
        className="mt-4 p-3 bg-yellow-500 text-white rounded-full shadow-md hover:bg-yellow-600 transition-colors"
      >
        {currentLang === 'pt' ? 'Próxima Palavra' : 'Next Word'}
      </button>
    </div>
  );
};

// Syllable Separation Component
const SyllableSeparator = ({ currentLang }) => {
  const words = wordsData[currentLang].words;
  const [currentWord, setCurrentWord] = useState(null);
  const [syllables, setSyllables] = useState([]);
  const [feedback, setFeedback] = useState('');

  const generateNewWord = useCallback(() => {
    const randomWord = words[Math.floor(Math.random() * words.length)];
    setCurrentWord(randomWord);
    setSyllables(randomWord.syllables);
    setFeedback('');
  }, [words]);

  useEffect(() => {
    generateNewWord();
  }, [generateNewWord]);

  const handleSyllableClick = (syllable) => {
    speak(syllable, currentLang);
  };

  if (!currentWord) return null;

  return (
    <div className="flex flex-col items-center p-6 bg-red-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto">
      <h2 className="text-3xl font-bold text-red-800 mb-6">
        {currentLang === 'pt' ? 'Separar em Sílabas' : 'Syllable Separation'}
      </h2>
      <div className="mb-6">
        <p className="text-xl text-red-700 mb-2">
          {currentLang === 'pt' ? 'Clique nas sílabas para ouvi-las:' : 'Click on the syllables to hear them:'}
        </p>
        <div className="flex flex-wrap justify-center gap-3 p-4 bg-red-200 rounded-lg shadow-inner">
          {syllables.map((syllable, index) => (
            <button
              key={index}
              onClick={() => handleSyllableClick(syllable)}
              className="p-3 bg-red-400 text-white rounded-lg shadow-md hover:bg-red-500 transition-colors text-2xl font-bold"
            >
              {syllable}
            </button>
          ))}
        </div>
      </div>
      <button
        onClick={generateNewWord}
        className="mt-4 p-3 bg-red-500 text-white rounded-full shadow-md hover:bg-red-600 transition-colors"
      >
        {currentLang === 'pt' ? 'Próxima Palavra' : 'Next Word'}
      </button>
    </div>
  );
};

// Reading Practice Component
const ReadingPractice = ({ currentLang }) => {
  const words = wordsData[currentLang].words;
  const [currentWord, setCurrentWord] = useState(null);
  const [syllables, setSyllables] = useState([]);

  const generateNewWord = useCallback(() => {
    const randomWord = words[Math.floor(Math.random() * words.length)];
    setCurrentWord(randomWord);
    setSyllables(randomWord.syllables);
    speak(randomWord.word, currentLang); // Speak the full word immediately
  }, [words]);

  useEffect(() => {
    generateNewWord();
  }, [generateNewWord]);

  const handleSpeakFullWord = () => {
    if (currentWord) {
      speak(currentWord.word, currentLang);
    }
  };

  const handleSpeakSyllable = (syllable) => {
    speak(syllable, currentLang);
  };

  if (!currentWord) return null;

  return (
    <div className="flex flex-col items-center p-6 bg-orange-100 rounded-xl shadow-lg w-full max-w-2xl mx-auto">
      <h2 className="text-3xl font-bold text-orange-800 mb-6">
        {currentLang === 'pt' ? 'Praticar Leitura' : 'Reading Practice'}
      </h2>
      <div className="mb-6 flex flex-col items-center">
        <p className="text-xl text-orange-700 mb-2">
          {currentLang === 'pt' ? 'Leia a palavra:' : 'Read the word:'}
        </p>
        <div className="flex flex-wrap justify-center gap-2 p-4 bg-orange-200 rounded-lg shadow-inner">
          {syllables.map((syllable, index) => (
            <span
              key={index}
              className="text-5xl font-extrabold text-orange-800 cursor-pointer hover:text-orange-600 transition-colors"
              onClick={() => handleSpeakSyllable(syllable)}
            >
              {syllable}
              {index < syllables.length - 1 && <span className="mx-1">-</span>}
            </span>
          ))}
        </div>
      </div>
      <button
        onClick={handleSpeakFullWord}
        className="flex items-center justify-center p-3 bg-orange-500 text-white rounded-full shadow-md hover:bg-orange-600 transition-colors mb-4"
      >
        <Volume2 size={32} />
        <span className="ml-2 text-xl">{currentLang === 'pt' ? 'Ouvir Palavra Completa' : 'Hear Full Word'}</span>
      </button>
      <button
        onClick={generateNewWord}
        className="p-3 bg-orange-500 text-white rounded-full shadow-md hover:bg-orange-600 transition-colors"
      >
        {currentLang === 'pt' ? 'Próxima Palavra' : 'Next Word'}
      </button>
    </div>
  );
};

// Main App Component
function App() {
  const [currentSection, setCurrentSection] = useState('alphabet'); // 'alphabet', 'wordFormation', 'syllableSeparation', 'readingPractice', 'dragLetter', 'colorLetter'
  const [currentLang, setCurrentLang] = useState('pt'); // 'pt' or 'en'

  const renderSection = () => {
    switch (currentSection) {
      case 'alphabet':
        return <AlphabetLearning currentLang={currentLang} />;
      case 'wordFormation':
        return <WordFormation currentLang={currentLang} />;
      case 'syllableSeparation':
        return <SyllableSeparator currentLang={currentLang} />;
      case 'readingPractice':
        return <ReadingPractice currentLang={currentLang} />;
      case 'dragLetter':
        return <DragLetterActivity currentLang={currentLang} />;
      case 'colorLetter':
        return <ColorLetterActivity currentLang={currentLang} />;
      default:
        return <AlphabetLearning currentLang={currentLang} />;
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-purple-200 to-indigo-300 p-4 font-sans flex flex-col items-center">
      <div className="w-full max-w-4xl bg-white rounded-2xl shadow-xl p-6">
        <h1 className="text-4xl font-extrabold text-center text-gray-800 mb-8">
          {currentLang === 'pt' ? 'Alfabetização Divertida!' : 'Fun Literacy!'}
        </h1>

        {/* Language Toggle */}
        <div className="flex justify-center mb-6 space-x-4">
          <button
            onClick={() => setCurrentLang('pt')}
            className={`px-6 py-3 rounded-full text-lg font-semibold transition-all duration-300 ${
              currentLang === 'pt'
                ? 'bg-indigo-600 text-white shadow-lg'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            }`}
          >
            Português
          </button>
          <button
            onClick={() => setCurrentLang('en')}
            className={`px-6 py-3 rounded-full text-lg font-semibold transition-all duration-300 ${
              currentLang === 'en'
                ? 'bg-indigo-600 text-white shadow-lg'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            }`}
          >
            English
          </button>
        </div>

        {/* Navigation Buttons */}
        <div className="flex flex-wrap justify-center gap-3 mb-8">
          <button
            onClick={() => setCurrentSection('alphabet')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'alphabet'
                ? 'bg-blue-600 text-white'
                : 'bg-blue-400 text-white hover:bg-blue-500'
            }`}
          >
            <BookOpen size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Alfabeto' : 'Alphabet'}
          </button>
          <button
            onClick={() => setCurrentSection('wordFormation')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'wordFormation'
                ? 'bg-yellow-600 text-white'
                : 'bg-yellow-400 text-white hover:bg-yellow-500'
            }`}
          >
            <Puzzle size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Formar Palavras' : 'Word Form'}
          </button>
          <button
            onClick={() => setCurrentSection('syllableSeparation')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'syllableSeparation'
                ? 'bg-red-600 text-white'
                : 'bg-red-400 text-white hover:bg-red-500'
            }`}
          >
            <Divide size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Sílabas' : 'Syllables'}
          </button>
          <button
            onClick={() => setCurrentSection('readingPractice')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'readingPractice'
                ? 'bg-orange-600 text-white'
                : 'bg-orange-400 text-white hover:bg-orange-500'
            }`}
          >
            <BookOpen size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Leitura' : 'Reading'}
          </button>
          <button
            onClick={() => setCurrentSection('dragLetter')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'dragLetter'
                ? 'bg-green-600 text-white'
                : 'bg-green-400 text-white hover:bg-green-500'
            }`}
          >
            <Puzzle size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Atividade: Arrastar' : 'Activity: Drag'}
          </button>
          <button
            onClick={() => setCurrentSection('colorLetter')}
            className={`flex items-center px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 shadow-md ${
              currentSection === 'colorLetter'
                ? 'bg-purple-600 text-white'
                : 'bg-purple-400 text-white hover:bg-purple-500'
            }`}
          >
            <Paintbrush size={18} className="mr-1" />
            {currentLang === 'pt' ? 'Atividade: Pintar' : 'Activity: Color'}
          </button>
        </div>

        {/* Render current section */}
        {renderSection()}
      </div>
    </div>
  );
}

export default App;
