---
layout: post
title: "Três meses de probabilidade e estatística (parte 2)"
date: 2020-12-01
categories:
  - Javascript
description:
image: https://source.unsplash.com/user/jdiegoph/GjAKNK4ocRc/2000x1200
image-sm: https://source.unsplash.com/user/jdiegoph/GjAKNK4ocRc/500x300
---

Para continuar meus estudo o vídeo do Curren Keller publicado no canal do youtube da freecodecamp foi de grande ajuda, através dele pude compreender o update pattern, mas também a trabalhar o código com componentes em javascript.

O update pattern definir uma forma de manter o DOM atualizado com relação aos dados carregados e que a cada interação do usuário o valor em página será atualizado sem qualquer problema. Para tal feito é preciso escrever código reusável e o menos acoplável possível, uma forma de realizar isso é dividir as partes que desenham cada parte do gráfico em diversas variáveis, assim essas mesmas partes serão atualizadas com apenas o alterar dos dados. Um método importante para tal feito é o merge().

~~~ javascript

    const groups = selection.selectAll(".tick").data(colorScale.domain());
    const groupsEnter = groups.enter().append("g").attr("class", "tick");

    groupsEnter
        .merge(groups)
        .attr("transform", (d, i) => {
        return `translate(${0}, ${i * spacing})`;
        })
        .attr("opacity", (d) => (!selectedRegion || d === selectedRegion ? 1 : 0.2)) 
        .on("click", (d, i) => onClick(d, i === selectedRegion ? null : i));

    groupsEnter
        .append("circle")
        .merge(groups.select("circle"))
        .attr("r", circleRadius)
        .attr("fill", colorScale);

    groupsEnter
        .append("text")
        .merge(groups.select("text"))
        .html((d) => d)
        .attr("dy", "0.32em")
        .attr("x", textOfSet);


~~~
<br>


O uso de componentes foi outra parte importante do processo de aprendizado, eu já tinha experiência com componentes visto que trabalho com angularJs e React, mas ver seu uso com d3js me deu uma nova percepção de como componentes podem serem poderosos.

Em resumo usei o modo como o react faz para criar componentes, ou pelo menos em parte. Os componentes foram declarados como funções javascript que recebem um objeto como dependecy injection.

~~~ javascript

    export function histoChart(svg, props) {
    const { width, height, margin, incomingData } = props;
    const innerWidth = width - margin.left - margin.right;
    const innerHeight = height - margin.top - margin.bottom;

    let xValue = (d) => xScale(d.release_year);
    let xPosition = (d) => xScale(d.x0);
    let yValue = (d) => yScale(d.length);

    let xScale = createXScale(incomingData, innerWidth);

    let h = createHistogramLayout();

    let bins = h(incomingData);
    let maxBin = max(bins, (d) => d.length);

    let yScale = createYScale(maxBin, innerHeight);

    let xAxis = createXAxis(xScale, innerHeight);
    let yAxis = createYAxis(yScale, maxBin);

    let gGroup = svg
        .append("g")
        .attr("class", "gGroup")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

    let xAxisG = gGroup
        .append("g")
        .attr("class", "xAxis")
        .attr("transform", `translate(${0}, ${innerHeight})`)
        .call(xAxis);

    let yAxisG = gGroup
        .append("g")
        .attr("class", "yAxis")
        .attr("transform", `translate(${0}, 0)`)
        .call(yAxis);

    gGroup
        .selectAll("rect")
        .data(bins)
        .enter()
        .append("rect")
        .attr("class", "bar-graph")
        .attr("x", xPosition)
        .attr("y", yValue)
        .attr("width", 10)
        .attr("height", (d) => innerHeight - yScale(d.length));
    }


~~~
<br>


Depois também utilizei estados pata alterar valores no DOM declarados externamente, e é nesse aspecto que o uso de componentes se mostrou tão surpreendente em seu uso, os componentes me possibilitaram alterar uma variável global em função da ação do usuário, além de que o conceito de estado impediu que houvesse descontrole na atualização dessa variável.

~~~ javascript

    export const mainChart = (selection, props) => {
        const { countries, innerWidth, innerHeight, margin } = props;
        let g = selection.append("g").attr("transform", `translate(${0}, ${0})`);
        let selectedRegion; // nossa variável externa

        //..................

        const executeRender = () => {
            g.call(render, {
            pathGenerator,
            scaleColor,
            colorValue,
            selectedRegion, // A variável é repassada para o componente render
            renderLegend,
            countries,
            });
        };

        const onClick = (d, i) => { // Declaramos aqui a função que responderá a interação do usuário
            selectedRegion = i; 
            executeRender(); // O modo como a variável será atualizada fica toda a cargo do componente render e suas dependências  
        };

        //......................

    };

~~~
<br>

Abaixo código que altera a variável externa.

~~~ javascript

    export const render = (g, props) => {
        const {
            pathGenerator,
            scaleColor,
            colorValue,
            selectedRegion, // variável externa recebida
            renderLegend,
            countries,
        } = props;

        const sphere = g.selectAll(".sphere").data([null]);
        const paths = g.selectAll(".country").data(countries.features);

        sphere
            .enter()
            .append("path")
            .attr("class", "sphere")
            .attr("d", pathGenerator({ type: "Sphere" }));

        sphere.merge(sphere).attr("opacity", selectedRegion ? 0.1 : 1);

        const path = paths.enter().append("path").attr("class", "country");

        path.append("title").text((d) => d.properties.name + ": " + colorValue(d));

        path
            .merge(paths)
            .attr("d", pathGenerator)
            .attr("fill", (d) => scaleColor(colorValue(d)))
            .attr("opacity", (d) =>
            !selectedRegion || selectedRegion === colorValue(d) ? 1 : 0.2 // variável externa usada para alterar o componente  
            )
            .classed(
            "highlighted",
            (d) => selectedRegion && selectedRegion === colorValue(d)
            );

        renderLegend();
    };


~~~
<br>

Tudo isso foi só para mostrar o quanto o conceito de interação e renderização é prático.
O próximo passo para mim foi tentar encontrar uma forma mais eficiente de comunicar esses dados, pois trabalhar com gráficos pouquinho mais complexo exigem uma atenção maior do que apenas medi-los e publicá-los na página web. No próximo artigo explico como amplie mais meus conhecimentos entendendo como estética e estatística se cruzam.