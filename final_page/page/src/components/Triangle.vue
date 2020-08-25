<template>
  <div id="body" width="1280px" height="800px">
  </div>
</template>

<script>
import response from "../../src/assets/temp.json";
import * as d3 from "d3";
export default {
  name: "Triangle",
  props: {},
  mounted() {
    this.buildLayout();
  },
  methods: {
    buildLayout() {
      console.log(response, "ABC");

      var codeColors = {
        "100": "#00ab7c", // ทหาร
        "010": "#ff0017", // ทักษิณ
        "001": "#36bed9", // ปชป
        "110": "#923e1b", // ทหาร + ทักษิณ
        "101": "#358295", // ทหาร + ปชป
        "011": "#7168ca", // ทักษิณ + ปชป
        "111": "#000000", // ทหาร + ทักษิณ + ปชป
      };

      var coreColors = {
        army: "#00ab7c",
        thaksin: "#ff0017",
        democrat: "#36bed9",
      };

      var width = 1280;
      var height = 800;
      var TriHeight = 553;
      var TriWidth = 645;
      var leftOffset = 315;
      var topOffset = 90;

      // create svg, required mapping objects
      var svg = d3
        .select("#body")
        .append("svg")
        .style("display", "block")
        .attr("width", width + "px")
        .attr("height", height + "px");
      console.log(svg);

      var centroid_x =
        (TriWidth / 2 + leftOffset + (TriWidth + leftOffset) + leftOffset) / 3;
      var centroid_y =
        (topOffset + (TriHeight + topOffset) + (TriHeight + topOffset)) / 3;

      // svg.append("line")
      //     .attr("x1", centroid_x)
      //     .attr("y1", centroid_y)
      //     .attr("x2", parseInt(TriWidth / 2 + leftOffset))
      //     .attr("y2", topOffset)
      //     .attr("stroke", stroke_color)
      //     .attr("stroke-width", stroke_width)
      //     .attr("stroke-dasharray", stroke_dash)

      // svg.append("line")
      //     .attr("x1", centroid_x)
      //     .attr("y1", centroid_y)
      //     .attr("x2", TriWidth + leftOffset)
      //     .attr("y2", TriHeight + topOffset)
      //     .attr("stroke", stroke_color)
      //     .attr("stroke-width", stroke_width)
      //     .attr("stroke-dasharray", stroke_dash)

      // svg.append("line")
      //     .attr("x1", centroid_x)
      //     .attr("y1", centroid_y)
      //     .attr("x2", leftOffset)
      //     .attr("y2", TriHeight + topOffset)
      //     .attr("stroke", stroke_color)
      //     .attr("stroke-width", stroke_width)
      //     .attr("stroke-dasharray", stroke_dash)

      svg
        .append("image")
        .attr("xlink:href", "../../src/assets/images/core_green.png")
        .style("position", "absolute")
        .attr("width", "54px")
        .attr("x", 234)
        .attr("y", 639);
      svg
        .append("image")
        .attr("xlink:href", "../../src/assets/images/core_red.png")
        .style("position", "absolute")
        .attr("width", "98px")
        .attr("x", 974)
        .attr("y", 641);
      svg
        .append("image")
        .attr("xlink:href", "../../src/assets/images/core_blue.png")
        .style("position", "absolute")
        .attr("width", "98px")
        .attr("x", 589)
        .attr("y", 27);
      svg
        .append("image")
        .attr("xlink:href", "~/src/assets/images/triangle.svg")
        .attr("id", "overlay-triangle")
        .style("position", "absolute")
        .attr("width", TriWidth)
        .attr("height", TriHeight)
        .attr("x", leftOffset - 0.3)
        .attr("y", topOffset - 0.3)
        .style("opacity", 0.8);

      svg
        .append("defs")
        .append("marker")
        .attr("id", "arrowhead")
        .attr("markerWidth", 7)
        .attr("markerHeight", 4)
        .attr("refX", 0)
        .attr("refY", 2)
        .attr("orient", "auto")
        .append("polygon")
        .attr("points", "0 0, 7 2, 0 4");
      svg
        .append("line")
        .attr("x1", 584 - 0.3)
        .attr("y1", 145)
        .attr("x2", 614 - 0.3)
        .attr("y2", 95)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");
      svg
        .append("line")
        .attr("x1", 692 - 0.3)
        .attr("y1", 145)
        .attr("x2", 662 - 0.3)
        .attr("y2", 95)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");
      svg
        .append("line")
        .attr("x1", 945 - 0.3)
        .attr("y1", 574)
        .attr("x2", 974 - 0.3)
        .attr("y2", 624)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");
      svg
        .append("line")
        .attr("x1", 893 - 0.3)
        .attr("y1", 661)
        .attr("x2", 952 - 0.3)
        .attr("y2", 661)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");
      svg
        .append("line")
        .attr("x1", 386 - 0.3)
        .attr("y1", 661)
        .attr("x2", 327 - 0.3)
        .attr("y2", 661)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");
      svg
        .append("line")
        .attr("x1", 333 - 0.3)
        .attr("y1", 574)
        .attr("x2", 304 - 0.3)
        .attr("y2", 624)
        .style("stroke", "black")
        .style("stroke-width", 1.5)
        .style("marker-end", "url(#arrowhead)");

      var scene_ids = new Set([]);
      var animate_data = {};
      var scene2apps = {}; // maps scene -> person -> app
      var firstapps = {}; // maps person -> 1st app
      response.forEach(function (d) {
        var scene_id = d.scene.split("_")[0];
        scene_ids.add(scene_id);
        d.points.forEach(function (point) {
          if (!(point.data.name in animate_data)) {
            animate_data[point.data.name] = {};
          }
          animate_data[point.data.name][scene_id] = {
            cx: point.cx,
            cy: point.cy,
            value: point.data.value * 2.0,
            name: point.data.name,
            color: codeColors[point.data.code],
          };
        });

        // map scene -> person -> app
        if (!(scene_id in scene2apps)) {
          scene2apps[scene_id] = {};
        }
        d.points.forEach(function (d) {
          scene2apps[scene_id][d.data.name] =
            animate_data[d.data.name][scene_id];
          // 1st apps
          if (!(d.data.name in firstapps)) {
            firstapps[d.data.name] = animate_data[d.data.name][scene_id];
          }
        });
      });
      scene_ids = Array.from(scene_ids).sort();

      console.log(scene_ids);
      console.log(animate_data);
      console.log(scene2apps);
      console.log(firstapps);

      Object.keys(animate_data).forEach(function (name) {
        var apps = animate_data[name];
        var circle = svg
          .append("circle")
          .attr("id", "circle-" + name.split(" ").join("-"))
          .style("visibility", "hidden")
          .style("opacity", 0.0);
        scene_ids.forEach(function (scene_id, i) {
          if (scene_id in apps) {
            var cx, cy;
            var d = apps[scene_id];
            var dx = centroid_x - d["cx"];
            var dy = centroid_y - d["cy"];
            if (d.color == codeColors["100"]) {
              cx = d["cx"] + dx * 0.02;
              cy = d["cy"] + dy * 0.02;
              cy += 2.0;
            } else {
              cx = d["cx"] + dx * 0.01;
              cy = d["cy"] + dy * 0.01;
            }
            // normalize for scene
            cx = ((cx - 60) / 690) * TriWidth + leftOffset;
            cy = ((cy - 60) / 590) * TriHeight + topOffset;
            d["cx"] = cx;
            d["cy"] = cy;
            var dur = 500;
            circle
              .transition()
              .delay(dur * i)
              .duration(dur)
              .attr("cx", cx)
              .attr("cy", cy)
              .attr("r", d["value"])
              .style("fill", d["color"])
              .style("visibility", "visible")
              .style("opacity", 1.0);
          }
        });
      });

      // animations
      var current_val = 53;
      var in_animation = true;

      function endAnimation() {
        in_animation = false;
      }
      window.setTimeout(endAnimation, 500 * 9 + 1000);

      // slider bar
      var slider = svg
        .append("g")
        .attr("class", "slider")
        .attr("transform", "translate(337,725)");

      var _range = [53, 54, 55, 56, 57, 58, 59, 60, 61, 62];
      var slideLength = 577;

      slider
        .append("line")
        .attr("class", "track")
        .attr("x1", 0)
        .attr("x2", slideLength)
        .select(function () {
          return this.parentNode.appendChild(this.cloneNode(true));
        })
        .attr("class", "track-inset")
        .select(function () {
          return this.parentNode.appendChild(this.cloneNode(true));
        })
        .attr("class", "track-overlay")
        .call(
          d3
            .drag()
            .on("start.interrupt", function () {
              slider.interrupt();
            })
            .on("start drag", function () {
              var val = d3.event.x / (slideLength / 9.0);
              val = Math.max(0, Math.min(Math.round(val), 9));
              update(val);
            })
        );

      var handle = slider
        .insert("circle", ".track-overlay")
        .attr("class", "handle")
        .attr("r", 16);
      for (var i = 1; i <= 9; i++) {
        handle
          .transition()
          .delay(500 * i)
          .duration(500)
          .attr("cx", i * (slideLength / 9));
        current_val = 53 + i;
      }

      slider
        .insert("g", ".track-overlay")
        .attr("class", "ticks")
        .selectAll("text")
        .data(_range)
        .enter()
        .append("text")
        .attr("x", (d) => (d - 53) * (slideLength / 9.0))
        .attr("y", 32)
        .style("font-size", "12px")
        .style("font-family", "NotoSans")
        .attr("text-anchor", "middle")
        .text(function (d) {
          return d;
        });

      function update(h) {
        if (!in_animation & (current_val !== 53 + h)) {
          handle.attr("cx", h * (slideLength / 9));

          // transition points
          var pt;
          var old_points = scene2apps[current_val];
          var new_points = scene2apps[53 + h];
          Object.keys(old_points).forEach(function (name) {
            if (name in new_points) {
              // only transition location
              pt = new_points[name];
              d3.select("#circle-" + name.split(" ").join("-"))
                .transition()
                .duration(500)
                .attr("cx", pt.cx)
                .attr("cy", pt.cy)
                .attr("r", pt.value)
                .style("fill", pt.color)
                .style("visibility", "visible")
                .style("opacity", 1.0);
            } else {
              // set visibility & opacity to 0
              pt = firstapps[name];
              d3.select("#circle-" + name.split(" ").join("-"))
                .transition()
                .duration(500)
                .attr("r", pt.value)
                .style("fill", pt.color)
                .style("visibility", "hidden")
                .style("opacity", 0.0);
            }
          });
          Object.keys(new_points).forEach(function (name) {
            if (!(name in old_points)) {
              // set visibility & opacity to 1
              var pt = new_points[name];
              d3.select("#circle-" + name.split(" ").join("-"))
                .transition()
                .duration(500)
                .attr("cx", pt.cx)
                .attr("cy", pt.cy)
                .attr("r", pt.value)
                .style("fill", pt.color)
                .style("visibility", "visible")
                .style("opacity", 1.0);
            }
          });
          current_val = 53 + h;
          addMouseover(current_val);
        }
      }

      // circle mouseover
      var avatar_radius = 24;
      var avatar_yoff;
      var defs = svg.append("defs");

      function addMouseover(scene_id) {
        Object.keys(animate_data).forEach(function (name) {
          d3.select("#circle-" + name.split(" ").join("-"))
            .on("mouseover", function () {})
            .on("mouseleave", function () {});
        });

        var apps = scene2apps[scene_id];
        Object.keys(apps).forEach(function (name) {
          var item = apps[name];
          if (item.value > 2) {
            var circle = d3.select("#circle-" + name.split(" ").join("-"));
            avatar_yoff = avatar_radius + parseInt(circle.attr("r")) + 4;
            circle.on("mouseover", function () {
              // mouse over
              // circle border
              circle.style("stroke", "black").style("stroke-radius", 4);
              // image avatar
              defs
                .append("clipPath")
                .attr("id", "avatar-clip" + name.split(" ").join("-"))
                .append("circle")
                .attr("cx", item.cx)
                .attr("cy", item.cy - avatar_yoff)
                .attr("r", avatar_radius);
              svg
                .append("circle")
                .attr("id", "image-avatar-bg")
                .attr("cx", item.cx)
                .attr("cy", item.cy - avatar_yoff)
                .attr("r", avatar_radius + 2)
                .attr("fill", "white");
              svg
                .append("image")
                .attr(
                  "xlink:href",
                  "../../src/assets/images/politician-photos-crop/" +
                    response[0].name2file[name.split(" ").join("-")]
                )
                .attr("id", "image-avatar" + name.split(" ").join("-"))
                .style("position", "absolute")
                .attr("width", parseInt(avatar_radius * 2) + "px")
                .attr("x", item.cx - avatar_radius)
                .attr("y", item.cy - avatar_yoff - avatar_radius)
                .attr(
                  "clip-path",
                  "url(#avatar-clip" + name.split(" ").join("-") + ")"
                );
              // text above
              var avatar_text_g = svg
                .append("g")
                .attr("id", "image-avatar-text-g")
                .attr(
                  "transform",
                  "translate(" +
                    item.cx +
                    "," +
                    (item.cy - avatar_yoff - avatar_radius - 15) +
                    ")"
                );
              var text_bg = avatar_text_g
                .append("rect")
                .attr("height", 24)
                .style("rx", 3)
                .style("ry", 3)
                .style("fill", "white");
              var text_name = avatar_text_g
                .append("text")
                .text(name)
                .style("font-family", "NotoSans")
                .style("font-size", 12)
                .style("font-weight", "bold")
                .style("dominant-baseline", "middle");
              var nc = parseInt(animate_data[name][scene_id].value / 2);
              var text_n = avatar_text_g
                .append("text")
                .text(nc + " สมัย")
                .style("font-family", "NotoSans")
                .style("font-size", 12)
                .style("dominant-baseline", "middle");
              var len_name = text_name.node().getBoundingClientRect().width;
              var len_n = text_n.node().getBoundingClientRect().width;
              var len_total =
                10 + len_name + 5 + len_n + 5 + nc * 6 + (nc - 1) * 2 + 10;
              text_name.attr("x", parseInt(10 + (-1 * len_total) / 2));
              text_n.attr(
                "x",
                parseInt(10 + (-1 * len_total) / 2 + len_name + 5)
              );
              text_bg
                .attr("width", len_total)
                .attr("x", parseInt((-1 * len_total) / 2))
                .attr("y", -15 + 1)
                .style("stroke", "black")
                .style("stroke-width", 1);

              for (var i = 0; i < nc; i++) {
                avatar_text_g
                  .append("circle")
                  .attr("r", 3)
                  .attr("cy", 0)
                  .attr(
                    "cx",
                    parseInt(
                      10 +
                        (-1 * len_total) / 2 +
                        len_name +
                        5 +
                        len_n +
                        5 +
                        i * (6 + 2)
                    )
                  )
                  .attr("fill", coreColors[response[0].name2app[name][i]]);
              }
            });
            circle.on("mouseleave", function () {
              // mouse leave
              // remove border
              circle.style("stroke", "none");
              // remove image
              defs.selectAll("*").remove();
              d3.select("#image-avatar-bg").remove();
              d3.select("#image-avatar" + name.split(" ").join("-")).remove();
              // remove text above
              d3.select("#image-avatar-text-g").remove();
            });
          }
        });
      }

      function initMouseover() {
        addMouseover(62);
      }

      window.setTimeout(initMouseover, 500 * 9 + 1000);
    },
  },
};

// });
</script>

<style lang="scss" scoped>
@font-face {
  font-family: NotoSans;
  src: url("../../src/assets/NotoSansThai-hinted/NotoSansThai-Regular.ttf");
}

@font-face {
  font-family: TheMATTER;
  src: url("../../src/assets/TheMATTER/TheMATTER-Bold.otf");
}

text {
  font-family: "ProximaNova", Helvetica, Arial, sans-serif;
  font-size: 12px;
}

#overlay-triangle {
  top: 64.5px;
  left: 64.5px;
}

.track,
.track-inset,
.track-overlay {
  stroke-linecap: round;
}

.track {
  stroke: #000;
  stroke-opacity: 0.3;
  stroke-width: 8px;
}

.track-inset {
  stroke: #dcdcdc;
  stroke-width: 8px;
}

.track-overlay {
  pointer-events: stroke;
  stroke-width: 50px;
  stroke: transparent;
  cursor: crosshair;
}

.handle {
  fill: #fff;
  stroke: #000;
  stroke-opacity: 0.5;
  stroke-width: 1.25px;
}
</style>