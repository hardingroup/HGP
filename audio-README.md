# Audio for The Long Road

Drop recordings into this folder with exactly these names and the game plays them. Nothing else needs changing: until a file exists, the game falls back to the browser's own voice.

- Format: `.mp3`, mono is fine. Trim the silence off both ends.
- Names are lower case, exactly as listed.
- The game looks for the file the first time it needs it. A missing file costs one failed request, then the stand-in voice takes over for the rest of that session.

## 1. The sounds (most important)

One clean recording of each sound **on its own**: `sss`, not `suh`; `mmm`, not `muh`; a short clipped `t`, not `tuh`. This is the part a computer voice gets wrong, and the part the science of reading depends on. Until a file exists the stand-in voice says, for example, "the first sound in sun".

The first 25 are used by the eight Hollow lessons. The rest complete the set for later lessons. There are 47 files for the 44 sounds of English, because c and k are the same sound under two letters, and x and qu are each two sounds a child meets as one letter.

| File | The sound in |
|---|---|
| `ph_s.mp3` | sun |
| `ph_a.mp3` | apple |
| `ph_t.mp3` | tap |
| `ph_p.mp3` | pin |
| `ph_i.mp3` | in |
| `ph_n.mp3` | net |
| `ph_m.mp3` | moon |
| `ph_d.mp3` | dog |
| `ph_g.mp3` | gate |
| `ph_o.mp3` | octopus |
| `ph_c.mp3` | cat |
| `ph_k.mp3` | kite |
| `ph_e.mp3` | egg |
| `ph_u.mp3` | up |
| `ph_r.mp3` | red |
| `ph_h.mp3` | hat |
| `ph_b.mp3` | bed |
| `ph_f.mp3` | fish |
| `ph_l.mp3` | leg |
| `ph_z.mp3` | zip |
| `ph_sh.mp3` | ship |
| `ph_ch.mp3` | chip |
| `ph_th.mp3` | thin |
| `ph_ai.mp3` | rain |
| `ph_oa.mp3` | boat |

Not used yet:

| File | The sound in |
|---|---|
| `ph_j.mp3` | jam |
| `ph_v.mp3` | van |
| `ph_w.mp3` | wet |
| `ph_x.mp3` | the end of fox |
| `ph_y.mp3` | yes |
| `ph_qu.mp3` | queen |
| `ph_th_v.mp3` | this |
| `ph_ng.mp3` | the end of ring |
| `ph_zh.mp3` | the middle of treasure |
| `ph_ee.mp3` | see |
| `ph_igh.mp3` | night |
| `ph_oo.mp3` | moon |
| `ph_oo_short.mp3` | book |
| `ph_ar.mp3` | car |
| `ph_or.mp3` | fork |
| `ph_ur.mp3` | fur |
| `ph_ow.mp3` | cow |
| `ph_oi.mp3` | coin |
| `ph_air.mp3` | chair |
| `ph_ear.mp3` | near |
| `ph_ure.mp3` | sure |
| `ph_er.mp3` | the end of butter |

## 2. Whole words (optional)

The stand-in voice reads these acceptably, so record them only if you want one voice throughout. Each is just the word, said clearly once.

`w_ant.mp3`, `w_apple.mp3`, `w_bad.mp3`, `w_bath.mp3`, `w_bed.mp3`, `w_bell.mp3`, `w_bone.mp3`, `w_bud.mp3`, `w_bug.mp3`, `w_cake.mp3`, `w_can.mp3`, `w_cane.mp3`, `w_cap.mp3`, `w_cat.mp3`, `w_chick.mp3`, `w_chin.mp3`, `w_chip.mp3`, `w_chop.mp3`, `w_cot.mp3`, `w_cow.mp3`, `w_cub.mp3`, `w_cube.mp3`, `w_cup.mp3`, `w_cut.mp3`, `w_dog.mp3`, `w_egg.mp3`, `w_fish.mp3`, `w_fox.mp3`, `w_goat.mp3`, `w_hat.mp3`, `w_hen.mp3`, `w_insect.mp3`, `w_jam.mp3`, `w_kit.mp3`, `w_kite.mp3`, `w_leg.mp3`, `w_log.mp3`, `w_map.mp3`, `w_moon.mp3`, `w_moth.mp3`, `w_mud.mp3`, `w_nap.mp3`, `w_net.mp3`, `w_nose.mp3`, `w_nut.mp3`, `w_orange.mp3`, `w_pan.mp3`, `w_peg.mp3`, `w_pie.mp3`, `w_pig.mp3`, `w_pin.mp3`, `w_pine.mp3`, `w_pit.mp3`, `w_pug.mp3`, `w_rope.mp3`, `w_sap.mp3`, `w_sat.mp3`, `w_shell.mp3`, `w_ship.mp3`, `w_shop.mp3`, `w_sit.mp3`, `w_sock.mp3`, `w_sun.mp3`, `w_tap.mp3`, `w_tent.mp3`, `w_tin.mp3`, `w_tip.mp3`, `w_top.mp3`, `w_umbrella.mp3`

## 3. The story (optional)

Played by the "Read it to me" button on each page of *The fox and the jam*.

- `story_1.mp3`: A fox came to the shop. The fox had a red cape.
- `story_2.mp3`: He had no cash. He hid a pot of jam in his cape.
- `story_3.mp3`: Fen ran up the lane. “Stop, fox!”
- `story_4.mp3`: The fox fell in the mud. The jam pot broke. Jam was on his nose.
- `story_5.mp3`: The fox felt bad. He made Fen a bench. Then Fen was glad.

## Not recorded

The teacher's own instructions ("Listen. Which one starts with this sound?") are spoken by the stand-in voice. They are the natural job for the AI teacher's voice when that is built, so they have no file names yet.
